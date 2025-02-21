---
상위 링크: "[[Javascript]]"
---
# Equality comparisons and sameness
Javascript는 다음 세 가지 값 비교 연산을 제공한다.

* `===` :  Strict Equality
* `==` : Loose Equality
* `Object.is()`

이들의 차이는 비교에 있어서 유형 변환 혹은 특수 처리가 발생하는지에 관련되어있다.

## Strict Equality
엄격 동등 비교는 두 값을 비교하기 전 어떠한 값도 암시적으로 변경하지 않는다. 값의 형식이 다른 경우, 두 값은 동등하지 않다고 판단한다!
```javascript
const num = 0;
const obj = new String('0');
const str = '0';

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false

console.log(null === undefined); // false;
console.log(num === Nan); // false

console.log(null === null); // true
console.log(undefined === undefined); // true

console.log(Nan === Nan); // false;
```

숫자를 제외한 모든 값에 대해 '하나의 값은 오직 자신과만 같다' 라는 명백한 의미체계를 제공한다는 점에서, 엄격한 동등은 거의 항상 사용하는 올바른 비교 연산이다. 특별히 수 타입에 관하여는 예외가 있다. **NaN에 대해서는 다른 모든 값들과 같지 않은 것으로 취급한다.**

엄격한 동등 방식은 `Array.prototype.indexOf()`, `Array.prototype.lastIndexOf()`, `case` 문 등에서 비교 시 사용된다.

## Loose Equality
느슨한 동등 비교는 두 값을 비교할 때에 형식이 다르다면, 암시적으로 비교가능한 형식으로 변환한다 *coercion*.

```javascript
const num = 0;
const big = 0n;
const str = "0";
const obj = new String("0");

console.log(num == str); // true
console.log(big == num); // true
console.log(str == big); // true

console.log(num == obj); // true
console.log(big == obj); // true
console.log(str == obj); // true
```

규칙은 다음과 같다.
1. 만약 형식이 동일하다면
	1. 객체: 동일한 객체를 참조하는 경우에 true
	2. 문자열 : 동일한 순서로 동일한 문자를 갖는 경우에만 true
	3. 숫자 : 값이 같은 경우 true (둘 중 하나가 NaN이면 false)
2. 피연산자 중 하나가 null이거나 undefined인 경우, 다른 피연산자도 null이거나 undefined이라면 true
	1. 즉, null == undefined는 true
3. 두 피연산자의 타입이 다르다면, 강제 형변환 후 1번 규칙대로 반환
	1. 불리언은 숫자로 변환
		1. true는 1, false는 0
	2. 문자열은 숫자로 변환
		1. Ex. '123' => 123
		2. 변환에 실패한다면 NaN

## Object.is()
#추후작성