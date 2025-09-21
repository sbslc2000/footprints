---
상위 개념: "[[Node.js]]"
---
# Node.js Event Loop

이벤트 루프는 Node.js가 기본적으로 단일 Javascript 스레드만 사용함에도 불구하고 논블로킹 I/O 작업을 수행할 수 있도록 해주는 매커니즘이다. 이는 작업을 시스템 커널로 위임함으로써 이루어진다.

대부분의 현대 커널은 멀티스레드이므로, 백그라운드에서 여러 작업을 동시에 처리할 수 있다. 이러한 작업 중 하나가 완료되면 커널은 Node.js에게 알리고, 이후 해당 콜백이 poll 큐에 추가되어 실행될 준비를 마친다.

## Event Loop
![](https://i.imgur.com/uJX19Sp.png)

Node.js가 시작되면 이벤트 루프를 초기화하고, 주어진 스크립트를 처리한다. 이 과정에서 비동기 API 호출을 하거나, 타이머를 예약하거나, process.nextTick()을 호출할 수 있다. 이후 이벤트 루프가 처리되기 시작한다.

위 그림의 각 박스는 이벤트 루프의 단계(phase)라고 불린다. 각 단계에는 FIFO [큐](../../../Data%20Structure/Linear%20Data%20Structure/Queue.md)가 있으며, 콜백이 그 안에 저장된다. 

Node.js는 각 단계에 들어가면 해당 단계의 특정 작업을 수행한 뒤, 큐의 콜백들을 최대 실행 제한에 도달하거나 큐가 비워질 때 까지 실행한 이후 다음 단계로 넘어간다.

이 과정에서 새로운 작업이 추가될 수 있다. 예를 들어, poll 단계에서 처리되는 동안에도 커널에서 새로운 이벤트가 도착하여 poll 큐에 추가될 수 있다. 따라서 실행시간이 긴 콜백이 poll 단계를 오래 점유하면, 타이머 단계의 실행 시점이 지연될 수 있다. 

## Phases Overview
* timers: `setTimeout()`과 `setInterval()`로 예약된 콜백을 실행하는 단계
* pending callbacks: 다음 루프 반복으로 연기된 I/O 콜백을 실행하는 단계(executes I/O callbacks deferred to the next loop iteration)
* idle, prepare: 내부적으로만 사용되는 단계
* poll: 새로운 I/O 이벤트를 가져오고, I/O 관련 콜백을 실행하는 단계
* check: setImmediate() 콜백이 이 단계에서 실행된다.
* close callbacks: 일부 close 콜백이 실행된다. ex. `socket.on('close', ...)`

이벤트 루프의 각 반복 사이에서 Node.js는 비동기 I/O나 타이머 대기 작업이 남아있는지 확인하고, 없다면 정상적으로 종료한다.

Node.js 20 (libuv 1.45.0) 부터는 이벤트 루프 동작이 변경되어, 타이머가 poll 단계 이후에만 실행되도록 바뀌었다.

### timers
타이머는 콜백이 실행될 정확한 시간을 지정하는 것이 아니라, 지정된 임계치(threshold)가 지난 후에 콜백이 실행될 수 있음을 보장한다. 타이머 콜백은 지정된 시간이 지난 뒤 가능한 한 빨리 실행되지만, 운영체제의 스케쥴링이나 다른 콜백의 실행 때문에 지연될 수 있다.

poll 단계는 타이머의 실행 시점을 제어한다. 예를 들어, 100ms 뒤에 실행되도록 타이머를 예약하고, 동시에 비동기 파일 읽기를 시작했다고 가정하자. 파일 읽기는 95ms가 걸린다.

```javascript
const fs = require('node:fs');

function someAsyncOperation(callback) {
  // 이 작업은 완료까지 95ms가 걸린다고 가정
  fs.readFile('/path/to/file', callback);
}

const timeoutScheduled = Date.now();

setTimeout(() => {
  const delay = Date.now() - timeoutScheduled;
  console.log(`${delay}ms have passed since I was scheduled`);
}, 100);

// 95ms 걸리는 비동기 작업 실행
someAsyncOperation(() => {
  const startCallback = Date.now();
  // 10ms 동안 실행되는 작업 수행
  while (Date.now() - startCallback < 10) {
    // 아무 것도 하지 않음
  }
});
```

이벤트 루프가 poll 단계에 들어갈 때는 아직 fs.readFile()이 완료되지 않았으므로, 큐는 비어있다. 따라서 이벤트 루프는 가장 가까운 타이머의 임계치가 도달할 때 까지 남은 시간(ms)만큼 기다린다. 기다리는 동안 95ms가 지나고, fs.readFile()이 완료되며, 그 콜백이 poll 큐에 추가되어 실행된다. 콜백 실행이 끝나면 더 이상 콜백이 없으므로, 이벤트 루프는 타이머의 임계치가 도달했음을 확인하고 다시 timers 단계로 돌아가 타이머 콜백을 실행한다.

이 예제에서 타이머가 예약된 시점부터 실제 콜백 실행까지 총 105ms가 소요된다.

> [!info]
> 이벤트 루프가 poll 단계에 지나치게 오래 머무르는 것을 방지하기 위해, Node.js의 비동기 동작을 구현하는 C 라이브러리(libuv)는 시스템 의존적인 최대 한계치(hard maximum)를 두어, 해당 시간 이후에는 더 이상 poll을 계속하지 않고 멈추도록 한다.


### Pending callbacks
이 단계에서는 일부 시스템 연산에 대한 콜백을 실행한다. 예를 들어 TCP 소켓이 연결을 시도할 때 ECONNREFUSED 오류를 받는 경우, 일부 유닉스 계열 시스템에서는 오류 보고를 지연시키기를 원한다. 이러한 경우 해당 콜백은 pending callbacks 단계에서 실행되도록 큐에 추가된다.

> [!info] ECONNREFUSED
> ECONNREFUSED는 연결이 거부되었음을 뜻하는 오류 코드이다. 클라이언트가 TCP 소켓을 통해 서버에 연결을 시도했지만, 해당 주소/포트에서 리스닝하고 있지 않거나, 방화벽 설정에 의해 연결이 즉시 거부된 경우를 말한다.
> 
> 이러한 오류는 즉시 발생하지만, 애플리케이션에 오류를 즉시 넘기지 않는데 이는 운영체제와 이벤트 루프의 일관성 때문이다. Node.js API는 기본적으로 항상 비동기로 동작한다는 철학을 갖기에, 에러가 즉시 발생하더라도 마치 비동기처럼 동작하는 것처러 보이게 해야 일관성을 유지할 수 있다. 만약 poll 단계에 콜백을 넣는다면, 이벤트 루프가 돌지 않고도 바로 콜백이 실행되어 일관성이 해쳐질 수 있다.

### Poll
Poll 단계에는 두 가지 주요한 기능이 있다.

1. 얼마 동안 I/O를 대기(block)하며 poll할 지 계산한다.
2. poll 큐에 있는 이벤트들을 처리한다.

이벤트 루프가 poll 단계에 들어갔을 때, 예약된 타이머가 없다면 두 가지 중 하나가 일어난다.

* poll 큐가 비어있지 않다면
이벤트 루프는 큐 안의 콜백들을 순차적으로(synchronously) 실행한다. 이때 큐가 모두 소진되거나, 시스템 의존적인 최대 한계치에 도달할 때 까지 실행된다.

* poll 큐가 비어있다면
만약 setImmediate()로 예약된 스크립트가 있다면, 이벤트 루프는 poll 단계를 끝내고 check 단계로 넘어가 예약된 스크립트를 실행한다.

만약 setImmediate()로 예약된 스크립트가 없다면, 이벤트 루프는 새로운 콜백이 큐에 추가할 때까지 대기하다가, 콜백이 들어오면 즉시 실행한다.

poll 큐가 비워지면 이벤트 루프는 임계치에 도달한 타이머가 있는지 확인한다. 하나 이상의 타이머가 준비되어 있으면, 이벤트 루프는 다시 timers 단계로 돌아가 해당 타이머들의 콜백을 실행한다.

### check
이 단계에서는 이벤트 루프가 poll 단계가 끝난 직후 콜백들을 실행할 수 있다. 만약 poll 단계가 유휴 상태가 되었고, setImmediate()로 예약된 스크립트가 있다면, 이벤트 루프는 기다리지 않고 곧바로 check 단계로 넘어가 그 스크립트들을 수행한다.

setImmediate()는 사실 이벤트 루프의 별도 단계에서 수행되는 특수한 타이머이다. 이는 libuv의 API를 사용하여, poll 단계가 끝난 뒤에 콜백이 실행되도록 예약한다.

일반적으로 코드가 실행되면 이벤트 루프는 결국 poll 단계에 도달하여, 들어오는 연결이나 요청등을 기다리게 된다. 그러나 만약 setImmediate()로 예약된 콜백이 있고, poll 단계가 유휴 상태가 되면, 이벤트 루프는 poll 이벤트를 기다리지 않고 poll 단계를 종료한 뒤 check 단계로 넘어가 예약된 콜백을 실행한다.

### close callbacks
소켓이나 핸들이 갑작스럽게 닫힌 경우 (ex. socket.destroy()) 'close' 이벤트는 이 단계에서 발생한다. 그렇지 않은 경우라면 process.nextTick()을 통해 발생한다.

