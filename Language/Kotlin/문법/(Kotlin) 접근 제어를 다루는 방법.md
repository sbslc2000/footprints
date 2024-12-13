---
상위 개념: "[[Kotlin 문법]]"
---
# 자바와 코틀린의 가시성 제어

Java에서는 4가지의 접근제어자가 있다, public, protected, default, private

Kotlin에서의 접근 제어자는 다음과 같다.
1. public: 모든 곳에서 접근 가능
2. protected : 선언된 클래스 또는 하위 클래스에서만 접근 가능
3. default -> internal : 같은 모듈에서만 접근 가능
	1. 코틀린은 패키지를 통해 접근제어를 하지 않는다.
	2. 모듈 : 한 번에 컴파일 되는 Kotlin 코드
4. private : 선언된 클래스 내에서만 접근 가능

Java에서는 기본 값이 default였지만, Kotlin에서는 기본 값이 public이다.
코틀린은 .kt 파일에 변수, 함수, 클래스 여러개를 바로 만들 수 있다.
# 코틀린 파일의 접근 제어

```kotlin
//kotlin.pt
```
* public은 기본값으로 어디서든 접근할 수 있다.
* protected: 파일 레벨에서는 사용할 수 없다.
* internal : 같은 모듈에서만 접근 가능하며
* private : 같은 파일 안에서만 접근할 수 있다.
# 다양한 구성요소의 접근제어

1. 멤버변수
	1. public
	2. protected
	3. internal
	4. private
2. 생성자
	1. 생성자에 접근 지시어를 붙이려면 constructor를 명시해야한다.

유틸 함수는 다음과 같이 사용하면 편하다

```kotlin
//StringUtils.kt
fun isDirectoryPath(path: String) : Boolean {
	return path.endsWith("/")
}

//java
StringUtils.isDirectoryPath("...")
```

3. 프로퍼티
```kotlin

	internal val name: String, //Getter와 Setter에 동시에

	var price = _price           //이 경우 getter는 public
		private set(value) = ...  //getter나 setter의 일부만 지정 가능 
```
# Java와 Kotlin을 함께 사용할 경우 주의할 점

Internal은 바이트 코드 상 public이 된다. 때문에 Java 코드에서는 Kotlin 모듈의 internal 코드를 가져올 수 있습니다.

Kotlin의 protected와 Java의 protected는 다르다. Java는 같은 패키지의 Kotlin protected 멤버에 접근할 수 있다.