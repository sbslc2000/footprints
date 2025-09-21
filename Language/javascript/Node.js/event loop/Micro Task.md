---
상위 개념: "[[Node.js Event Loop]]"
---
# Micro Task
마이크로 태스크(Micro Task)란 [매크로 태스크](Macro%20Task.md)가 끝난 직후, 다음 매크로 태스크로 넘어가기 전에 무조건 비워지는 작은 큐이다. Promise.then, queueMicrotask, MutationObserver 콜백들이 이 큐에 저장된다.

## fetch와 Micro Task Queue
fetch 요청을 하면 다음과 같은 방식으로 시스템이 동작한다.

1. 요청 발생 
fetch를 호출하면 내부적으로 Node.js는 libuv의 비동기 I/O 시스템을 사용해 소켓을 열고, 네트워크 스택을 통해 TCP 연결 -> HTTP 요청 전송 과정을 시작한다.

2. 이벤트 루프와 poll 단계
커널이 소켓에 데이터가 도착했음을 알리면, libuv가 이를 감지한다. 이 이벤트는 poll 단계의 큐에 들어간다.

이벤트 루프가 poll 단계에 들어가면, 해당 콜백을 실행하여 응답 데이터를 처리한다.

fetch는 Promise를 반환한다. 내부적으로 resolve(response)가 호출되며, 이와 연결된 .then() 콜백은 마이크로태스크 큐에 들어간다.

3. 마이크로태스크 큐 처리
매크로 태스크의 실행이 끝난 직후, 이벤트 루프는 마이크로태스크 큐의 콜백함수를 실행한다. 이 지점에서 .then() 콜백이 실행된다.