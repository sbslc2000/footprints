---
상위 링크: "[[Programming Language Theory]]"
---
# Short-Circuit Evaluation

> The Result is determined without evaluating all of the operands and/or operators

단락 평가 *Short-Circuit Evaluation* 이란, 식의 일부분만을 가지고 식의 전체를 평가할 수 있는 것을 의미한다. 
다음과 같은 if문이 있다고 가정해보자
```
if a == b or b == c {
	print("Hello")
}
```

위 if문의 조건은 a가 b가 동일하다면 `b==c` 에 대한 부분은 평가하지 않더라도 전체 식은 참이다. 단락 평가가 허용된 언어에서는 이 경우 뒤의 식을 평가하지 않으며, 단락평가가 허용되지 않은 언어에서는 전체 식을 평가한다.

몇몇 언어에서는 단락 평가가 전역 정책으로 설정되어 있다. Fortran와 C는 모든 논리식에서 단락평가를 허용한다. 하지만 몇몇 언어에서는 단락 평가를 개발자의 옵션으로 두기도 한다. Java와 Ada가 그 예이다.

```java
if (obj != null && obj.isInitiated()) {} //단락평가를 허용하는 구문으로, obj가 null이라면 두번째 식은 수행되지 않음

if (obj != null & obj.isInitiated()) {} // 단락평가를 비허용하는 구문으로, obj가 null인 경우에도 두번째 식이 수행됨
```