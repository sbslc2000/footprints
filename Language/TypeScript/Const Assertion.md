---
상위 링크: "[[Typescript]]"
---
# Const Assertions
```typescript
let user = {
	name: "Pete",
	age: 26,
} as const;
```
위와 같이 작성된다면, 모든 객체의 속성은 readonly가 적용된다.