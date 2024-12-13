---
상위 개념: "[[Kotlin 문법]]"
---
# 추상 클래스
```kotlin
abstract class Animal(
	protected val species: String,
	protected open val legCount: Int
) {
	abstract fun move()

}
```

```kotlin
class Cat(
	species: String
) : Animal(species,4) {

	override fun move() {
		println("냥냥")
	}
} 
```

```kotlin
class Penguin(
	species: String
) : Animal(species, 2) {

	private val wingCount: Int = 2

	override fun move() {
		println("펭펭")
	}

	override val legCount: Int
		get() = super.legCount + this.wingCount
}
```

* 상속은 extends 대신 : 을 사용한다.
* 상속 클래스를 지정하면서 생성자를 바로 호출한다.
* override를 필수적으로 붙여주어야 한다.
* 추상 프로퍼티가 아니라면 오버라이드를 위해서는 open을 붙여야 한다.

# 인터페이스
```kotlin
interface Flyable {
	fun act() {
		println("파닥 파닥")
	}
}

interface Swimmable {
	fun act() {
		println("파닥 파닥")
	}
}
```

```kotlin
class Penguin(
	species: String
) : Animal(species, 2), Swimmable, Flyable {
...
	override fun act() {
		super<Swimmable>.act()
		super<Flyable>.act()	
	}
}
```

* default를 작성하지 않아도 default 메소드 를 만들 수 있다.


backing field가 없는 프로퍼티를 Interface에 만들 수 있다.
```kotlin

interface Swimmable {
	val swimAbility: Int

	fun act() {
		println("어푸 어푸")
	}
}

class Penguin {
	override val swimAbility 
		get() = 3
}
```
# 클래스 상속할 때 주의할 점

```kotlin
open class Base(
	open val number: Int = 100
) {
	init {
		println("Base Class")
		println(number)
	}
}

class Derived(
	override val number: Int
) : Base(number) {
	init {
		println("Derived Class")
	}
}

fun main() {
	Derived(300)
}

//Base Class
//0
//Derived Class
```

상위 클래스 생성자가 수행되는 동안, 하뒤 
상위 클래스의 constructor나 init block에서는 하위 클래스에서 override하고 있는 프로퍼티에 접근하면 안된다.
따라서 상위 클래스를 설계할 때, 생성자 또는 초기화 블록에 사용되는 프로퍼티에는 open을 피해야한다.

# 상속 관련 지시어 정리

1. final : override를 할 수 없게 한다. default 값이다.
2. open: override를 열어준다.
3. abstract : 반드시 override 해야한다.
4. override : 상위 타입을 오버라이드 하고 있다.