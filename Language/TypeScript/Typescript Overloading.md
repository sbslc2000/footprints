---
상위 개념: "[[Typescript]]"
---
# Typescript Overloading
타입스크립트에서 오버로딩을 구현하려면 버전별 오버로드 시그니처를 만들어주어야 한다.

```typescript
//시그지처
function func(a: number): number;
function func(a: number, b: number): number;

//구현부
function func(a: number: b?: number) {
	if (b) {
		return a + b;
	}

	return a * 2;
}
```

구현 함수의 시그니처는 모든 오버로드 시그니처와 호환되도록 작성해야한다.