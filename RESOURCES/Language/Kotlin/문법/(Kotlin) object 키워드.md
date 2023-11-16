# static 함수와 변수
![](https://i.imgur.com/RLblkuT.png)

```kotlin

class Person private constructor(
	var name: String,
	var age: Int
) {
	companion object Factory : Log {
	
		override fun log() {
			println("Person")
		}
		
		private const val MIN_AGE = 1

		@JvmStatic
		fun newBaby(name: String): Person {
			return Person(name, MIN_AGE)
		}
	}
}


//java

//Person.Companion.newBaby("ABC");
Person.newBaby("ABC"); //단, @JvmStatic이 붙어있어야 함
Person.Factory.newBaby("ABC")// JvmStatic이 없다면 이름을 붙이거나 Companion 붙이기
```

kotlin은 static이 없다. -> companion object : 클래스와 동행하는 유일한 오브젝트
	동반 객체도 하나의 객체로 간주되어, 이름을 붙일 수 있으며 interface를 구현할 수도 있다.
	companion object에 유틸성 함수를 넣어도 되지만, 최상단 파일을 사용하는 것을 권장
const : 컴파일시에 변수 할당 -> 상수에 붙이기 위한 용도, 기본 타입과 String에 붙일 수 있음
# 싱글톤
```kotlin
//싱글톤으로 관리됨
object Singleton {
	var a: Int = 0
}

fun main() { //인스턴스화 할 필요 없음 
	Singleton.a += 10
	prinln(Singleton.a)
}
```
# 익명 클래스
특정 인터페이스나 클래스를 상속받은 구현체를 일회성으로 사용하는 것
![](https://i.imgur.com/te3LF19.png)

```kotlin

fun main() {
	moveSomethjing(object : Movable {
		override fun move() {
			println("뭅뭅")
		}
	})

}

fun moveSomething(movable: Movable) {
	movable.move();
}
```