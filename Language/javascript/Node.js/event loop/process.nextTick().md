---
상위 개념: "[[event loop/Node.js Event Loop]]"
---
# process.nextTick()
process.nextTick()은 이벤트 루프의 단계에 속하지 않는 Node.js 전용 큐 이다. 이 큐는 '지금 하고 있는 작업이 끝난 직후'에 실행할 콜백을 담고 있다. 

이벤트 루프는 각 단계마다 콜백을 하나 실행한 뒤, 다음 단계로 넘어가기 전에 내부적으로 다음 과정을 수행한다.

1. process.nextTick() 큐 비우기
2. 마이크로태스크(Promise.then\/queueMicrotask) 처리
3. 그 다음 Phase로 진행

즉 process.nextTick()은 phase 사이클보다도 앞서고, Promise 마이크로태스크보다도 우선한다.

```js
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
process.nextTick(() => console.log('nextTick'));

// 출력 순서(대개): nextTick → promise → timeout
```


## 예제 : EventEmitter
```javascript
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {
  constructor() {
    super();
    this.emit('event');
  }
}
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('an event occurred!');
});
```
위 코드는 생성자 안에서 이벤트를 즉시 발생시키므로, 사용자가 핸들러를 등록하기도 전에 이벤트가 발행되어 버린다.

```js
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {
  constructor() {
    super();
    // 생성자가 끝난 뒤 이벤트 발생
    process.nextTick(() => {
      this.emit('event');
    });
  }
}
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('an event occurred!');
});
```

이를 방지하기 위하여 생성자 안에 process.nextTick()을 사용하면, 생성자가 끝난 뒤 이벤트가 실행되므로 사용자가 핸들러를 등록할 기회를 가질 수 있다.

