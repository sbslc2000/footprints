---
상위 개념: "[[Kotlin 문법]]"
---
# 변수

## var과 val,  type
```kotlin
var num1 = 10L
num1 = 5L

val num2 = 10L
val num2 = 5L //컴파일 에러
```

가변 변수는 var(variable), 불변 변수는 val(value)을 사용한다.

또한 변수를 사용할 때 컴파일러가 타입을 자동으로 추론해주어서 작성하지 않아도 무방하지만, 원한다면 다음과 같이 작성해줄 수 있다.

```kotlin
var num1 : Long = 10L

var num2 // 컴파일러가 타입을 추론할 수 없으므로 에러

var num3 : Long // 타입을 지정해주었으므로 에러는 발생하지 않음
prinln(num3) // 사용시 초기화하지 않았다면 에러 발생

val num4
num4 = 10L // 이건 가능, 컴파일러가 num4 생성시 값을 초기화
```

## Primitive Type
**숫자, 문자, 불리언**과 같은 몇몇 타입은 외부적으로는 클래스처럼 보이지만 연산시에는 primitive type으로 바꾸어서 적절한 처리를 해준다. -> 프로그래머가 boxing과 unboxing을 고려하지 않아도 된다.

## Nullable
변수에 null을 넣고 싶다면 **타입?** 을 이용해야한다. 타입과 타입?는 아예 다른 타입으로 간주된다.
```kotlin
var number1 = 10L
var number3 : Long? = 10L
number3 = null
```

Nullable과 관련해서 컴파일 타임에 잡지 못하는 오류가 쉽게 발생하는 듯 하다.

```kotlin
//Controller.kt

...
@GetMapping(...)
fun someMethod(
	value: Int = 1
) {
 ...
}
```
Controller에서 RequestParam의 기본 값을 지정해줄 때 위와 같이 작성하면 문제가 발생하는데, 바인딩하는 과정에서 Int가 null인 경우 기본 값을 할당해주는 형태로 수행되기 때문에 value가 null일 가능성이 생기기 때문이다. 이 경우는 @RequestParam의 defaultValue 속성을 통해 문제를 해결할 수 있다.

```
## 객체 인스턴스화
new 키워드는 사용하지 않는다.
```kotlin
var person = Person("서범석")
```


