---
상위 개념: "[[Frontend]]"
---
# Hydration
Hydration이란 클라이언트 측 [자바스크립트](../Language/javascript/Javascript.md)가 정적인 웹 페이지(정적 렌더링이나 서버사이드 렌더링으로 전달된)를 동적으로 변환하는 기술이다. 이는 HTML 요소에 이벤트 핸들러를 DOM에 연결함으로써 이루어진다.

HTML이 서버에서 미리 렌더링되므로, 유저에게 빠른 첫 콘텐츠 렌더링(First Contentful Paint)를 제공할 수 있지만, 그 직후 페이지가 완전히 로드되고 상호작용이 가능한 것 처럼 보여도 실제로는 클라이언트 측 자바스크립트가 수행되고 나서 상호작용이 된다.

Hydration을 사용하는 프레임워크로는 Next.js와 Nust.js가 있으며, React v16.0부터는 hydrate 함수가 API에 도입되었다.

## Hydration Error
Hydration 오류는 서버에서 렌더링 된 HTML에 대하여 자바스크립트가 hydration하는 과정에서 DOM 구조나 상태가 불일치 할 때 발생한느 오류이다.