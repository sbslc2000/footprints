---
상위 개념: "[[React Hooks]]"
---
# useCallback
`useCallback`은 React에서 제공하는 훅(Hook)중 하나로, 메모이제이션된 콜백 함수를 반환해준다. 즉, 컴포넌트가 리렌더링될 때마다 함수를 새로 만드는 것을 방지하고, 필요할 때만 함수를 새로 생성하도록 돕는 역할을 한다.

## 기본 문법
```js
const memoizedCallback = useCallback(
	() => {}, [dependency1, dependency2]
);
```
첫번째 인자는 실행될 콜백 함수이며, 두번째 인자는 의존성 배열로 이 값들이 바뀔 때 새로운 함수가 생성된다.