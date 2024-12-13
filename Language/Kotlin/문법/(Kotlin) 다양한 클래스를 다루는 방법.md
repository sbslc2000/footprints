---
상위 개념: "[[Kotlin 문법]]"
---
# Data Class

```kotlin

data class PersonDto(
	val name: String,
	val age: Int,
)

fun main() {
	val p1 = PersonDto("서범석", 100)
	val p2 = PersonDto("서범석", 100)

	println(p1 == p2) // true
}
```

data class는  equals, hashcode, tostring을 제공한다. 여기에 named argument를 활용하면 builder pattern을 사용하는 것과 동일한 효과를 누릴 수 있다.

# Enum Class
![](https://i.imgur.com/cBiFcs6.png)

```kotlin

enum class Country(
	private val code: String,
) {
	KOREA("KO"),
	AMERICA("US")
	;

}
```

![](https://i.imgur.com/rbYr7Op.png)

```kotlin
fun handleCountry(country: Country) {
	when (country) {
		Country.KOREA -> ...
		Country.AMERICA -> ...
	}
}
```

enum이 추가됐을 때 ide레벨에서 알림을 주고, 컴파일러가 country를 모두 검사하기 때문에 else를 작성하지 않아도 된다.

# Sealed Class, Sealed Interface

sealed: 봉인을 한

상속이 가능하도록 추상클래스를 만들까 하는데, 외부에서는 이 클래스를 상속받지 않았으면 좋겠을 때
 -> 하위 클래스를 봉인

컴파일 타임 때 하위 클래스 타입을 모두 기억한다. 즉, 런타임에 클래스 타입이 추가될 수 없다. 하위 클래스는 같은 패키지에 있어야 한다.

Enum과 다른 점
- 클래스를 상속받을 수 있다.
- 하위 클래스는 멀티 인스턴스가 가능하다.

```kotlin
sealed class HyundaiCar(
	...
) 

class Avante : HyundaiCar(...)
class Sonata : HYundaiCar(...)
...
```

-> 컴파일 타임에 하위 클래스를 모두 기억하기 때문에, 런타임에 추가되지 않는다.

```kotlin

fun handleCar(car: HyundaiCar) {
	...
}
```

when을 사용할 때, ide 레벨의 알림과 else를 사용하지 않아도 된다.