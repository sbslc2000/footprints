
# 클래스와 프로퍼티
![](https://i.imgur.com/x0RLgaJ.png)


```kotlin
class Person (
	val name: String,
	age: Int)	
```
* 기본이 Public이므로 생략 가능
* 코틀린에서는 필드만 만들면 getter와 setter를 자동으로 만들어준다.
* constructor라는 지시어는 생략할 수 있다
* 생성자에서 프로퍼티를 만들 수 있다.
* body에 아무것도 없는 경우 생략할 수 있다
*

```kotlin
fun main() {
	val person = Person("서범석",100)
	println(person.name)
	person.age = 10
}
```

 * .필드를 통해 getter와 setter를 호출할 수 있다
 * Java 클래스에 대해서도 .필드를 통해 getter setter를 호출한다.

# 생성자와 init
```java
public JavaPerson(String name, int age) {
	if(this.age <= 0) {
		throw new IllegalArgumentException(...);
	}
	this.name = name;
	this.age = age;
}
```

```kotlin

fun main(){
	val Person("서범석")
}
class Person(
	val name: String,
	var age: Int
) {
	init {
		if(age <= 0)
			throw IllegalArgumentException(...)
	}

	constructor(name : String): this(name, 1)
}
```

* init(초기화) 블록은 생성자가 호출되는 시점에 호출된다.
* 아래에 constructor(parameter)를 통해 생성자를 추가적으로 만들 수 있으며, this를 통해 위에 있는 생성자를 호출 할 수 있다.
* 위에 있는 생성자는 Primary Constructor라고 불리며, 반드시 존재해야한다.
	* 단, 주 생성자에 파라미터가 하나도 없다면 생략 가능하다.
	* 부 생성자에서는 최종적으로 주 생성자를 this로 호출해야한다.
	* 부 생성자에는 body를 가질 수 있다.
* 부 생성자보다는 default parameter를 권장한다!
* converting과 같은 경우는 정적 팩토리 메소드를 추천한다. -> 사실상 부 생성자를 사용할 일이 없다.

# 커스텀 getter, setter
![](https://i.imgur.com/Dik5vX9.png)

```kotlin
class Person... {
...
	fun isAdult(): Boolean {
		return this.age >= 20
	}

	val isAdult: Boolean
		get() = this.age >= 20
//		 get() {
//			return this.age >= 20
//		} 
}
```
프로퍼티처럼 만들어서 사용할 수도 있다.

속성을 확인하고자 할 때는 custom getter를 하고, 무언가 동작하는 것이라면 메소드를 사용하는 것이 좋지 않을까?


## Custom Setter
```kotlin
class Person(
	name: String = "서범석",
	var age: Int = 1
) {
	var name = name
		set(value) {
			field = value.uppercase()
		}
}
```
setter 자체는 지양하기 때문에 크게 쓸 일이 없다.



# backing field

```kotlin
class Person(
	name: String = "서범석", //이렇게 하면 자체적으로 getter setter를 만들지 않는다.
	var age: Int = 1
) {
	
	val name = name
		get() = field.uppercase()

}
```

* name.uppercase() 로 사용한다면 무한 루프 발생
* 따라서 field로 바꾸어 주어야 한다. -> 필드 자기 자신을 가리킴 -> backing field

```kotlin
class Person(
	val name: String = "서범석", //이렇게 하면 자체적으로 getter setter를 만들지 않는다.
	var age: Int = 1
) {
	
	val uppercaseName: String
		get = name.uppercase()

}
```
이런 방법으로 작성하여도 문제 없다.