---
상위 링크: "[[Frontend]]"
---
# Service Worker
서비스 워커(Service Worker)는 웹 브라우저의 백그라운드에서 동ㅈ가하는 특별한 스크립트이다. 보통 자바스크립트는 웹 페이지가 열려 있을 때만 실행되지만, 서비스 워크는 **페이지와는 별도로 백그라운드에서 실행**되면서 네트워크 요청 가로채기, 캐시 제어, 푸시 알림 처리와 같은 기능을 제공한다.

Service Worker는 보안 상의 이유로 HTTPS 환경 (또는 개발 중의 localhost)에서만 동작한다.

## Life Cycle
서비스 워커는 다음과 같은 생명주기를 갖는다.

### 1. 등록(Registration)
```js
navigator.serviceWorker.register('/sw.js')
```
서비스 워커는 웹페이지에서 위와 같은 코드가 호출되어야 시작된다. 브라우저는 해당 스크립트를 다운로드하고, '서비스 워커 관리' 영역에 등록한다.

### 2. 설치(Installation)
새 서비스 워커가 처음 등록되거나, 코드가 바뀌었을 때 install 이벤트가 발생한다. 이 시점에 필요한 캐시를 미리 저장(pre-cache)하는 작업을 주로 한다. 설치가 성공하면 installed 상태가 되며, 기존 워커가 있다면 새 워커는 대기(waiting) 상태로 들어간다.
```js
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('v1').then(cache => cache.addAll(['/index.html', '/main.css']))
  );
});
```

### 3. 대기(Waiting)
이미 활성화된 워커가 있었다면, 새로 설치된 서비스 워커는 바로 교체되지 않고 대기 상태가 된다. 이는 페이지 실행 중에 갑자기 워커가 변경되면 리소스가 꼬일 수 있기 때문이다. 그래서 보통은 웹페이지를 닫았다가 다시 열 때 새로운 워커가 적용된다.

### 4. 활성화(Activate)
활성화는 기존 워커가 제거되고, 새로운 워커가 들어올 때 발생하는 이벤트입니다. 주로 옛날 캐시 정리 작업을 수행합니다.

```js
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then(keys => 
      Promise.all(keys.map(key => key !== 'v1' ? caches.delete(key) : null))
    )
  );
  self.clients.claim();
});
```

### 5. 동작 (Running)
활성화된 서비스 워커는 이제 fetch 이벤트나 push 이벤트 등을 처리할 수 있습니다.
```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then(res => res || fetch(event.request))
  );
});
```

### 6. 업데이트 (Update)
브라우저는 주기적으로 서비스 워커 파일을 다시 가져와 비교합니다. 이 때 만약 내용이 바뀌었다면, 다시 Install -> Waiting -> Activate 흐름을 밟습니다.