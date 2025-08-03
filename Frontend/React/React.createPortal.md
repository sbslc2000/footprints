---
상위 개념: "[[React]]"
---
# React.createPortal
React.createPortal은 리액트의 포탈(Portal) 기능을 제공하는 API로, 리액트 컴포넌트를 현재 컴포넌트의 DOM 계층이 아닌 다른 DOM 노드에 렌더링할 수 있게 해주는 함수이다.

## 문법
```typescript
import { createPortal } from 'react-dom';

createPortal(child: ReactNode, container: Element);
```
* child : 렌더링할 리액트 엘리먼트(JSX)
* container: 실제 DOM 노드 (HTMLElement)

## 예시
```typescript
import ReactDOM from 'react-dom';

function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById('modal-root')! // body 외부의 노드에 렌더링
  );
}
```
위 Modal의 children들은 model-root DOM 노드에 렌더링된다.