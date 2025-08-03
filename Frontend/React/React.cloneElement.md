---
상위 개념: "[[React]]"
---
# React.cloneElement
React.cloneElement는 기존의 React 엘리먼트를 복제하면서, 새로운 props를 추가하거나 변경할 수 있게 해주는 함수이다. 주로 컴포넌트를 덮어씌우거나 래핑하면서도 원래의 내용을 유지하고 싶을 때 사용한다.

## 문법
```typescript
React.cloneElement(element, [props], [...children])
```
* element: 복제할 React 요소 (JSX 엘리먼트)
* props: 덮어씌울 새로운 props
* children: 새롭게 지정할 자식 노드

## 예시
```typescript
const button = <button>Click Me</button>;

const cloned = React.cloneElement(button, {
	onClick: () => alert('클릭됨!');
})
```
이를 통해 button의 컴포넌트 자체를 수정하지 않으면서도 이벤트를 부여할 수 있다.