---
상위 개념: "[[Node.js Event Loop]]"
---
# setImmediate
setImmediate()란 Node.js 전용 API로, 현재 이벤트 루프 사이클의 poll 단계가 끝난 직후 check 단계에 콜백을 실행하도록 예약한다.

## vs. setTimeout()
`setImmediate()`와 `setTimeout()`은 유사하지만, 호출되는 시점에 따라 서로 다르게 동작한다.

* setImmediate는 poll 단계가 완료된 직후에 스크립트를 실행하도록 설계되었다.
* setTimeout()은 지정된 최소 임계 시간(ms)이 지난 뒤에 스크립트를 실행하도록 예약한다.

이 타이머들이 실행되는 순서는 호출된 맥락에 따라 달라진다. 

```javascript
// timeout_vs_immediate.js
setTimeout(() => {
  console.log('timeout');
}, 0);

setImmediate(() => {
  console.log('immediate');
});
```

만약 둘 다 메인 모듈(즉, I/O 사이클이 아닌 곳)에서 호출되면, 순서는 프로세스 성능(즉, 실행중인 다른 애플리케이션의 영향을 받을 수 있음)에 의해 결정되므로 비결정적이다.

```javascript
// timeout_vs_immediate.js
const fs = require('node:fs');

fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);

  setImmediate(() => {
    console.log('immediate');
  });
});
```
그러나 이 두 호출은 I/O 사이클 안으로 옮기면, 항상 setImmediate() 콜백이 먼저 실행된다.