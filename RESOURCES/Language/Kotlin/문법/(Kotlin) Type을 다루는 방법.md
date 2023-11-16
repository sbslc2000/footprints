# 기본 타입
Byte, Short, Int, Long, Float, Double  과 같은 Java에서 지원하는 타입이 Kotlin에도 그대로 존재한다.

## 타입 추론
코틀린에서는 선언된 기본 값을 보고 타입을 추론한다.
```kotlin
val number1 = 3 // Int
val number2 = 3L // Long
val number3 = 3.0f // Float
val number4 = 3.0 // Double
```
## 타입 변환
Java에서는 타입 변환이 암시적으로 수행되지만, Kotlin에서는 명시적으로 작성해주어야 한다.
```kotlin
val number1 = 4
val number2 : Long = number1 // Type mismatch error
val number2 : Long = number1.toLong() //ok
```
## nullable
null이 들어갈 수 있음을 표현하려면 타입? 를 사용한다.
```kotlin
val number1 : Int? = 3
val number2 : Long = number1?.toLong() ?: 0L
```
# 타입 캐스팅
객체 타입의 캐스팅은 어떻게 해야할까?
instance of 는 is 로, 캐스팅은 as 로 표현한다.
is의 반대는 !is로 표현하며 as 는 생략도 가능하다.

as? 는 Type 불일치시 null을 반환한다!
```kotlin
public static void printAgeIfPerson(Object obj) {
	if (obj instanceof Person) {
		Person person = (Person)obj;
		System.out.println(person.getAge());
	}
}

//kotlin으로 표현한다면
fun printAgeIfPerson(obj: Any) {
	if (obj is Person) {
		val person = obj as Person
		println(person.age)
	}
}

//타입 체크 시 코틀린은 obj를 person으로 간주하여 코드를 실행시킬 수 있다.
//따라서 as 생략 가능
fun printAgeIfPerson(obj: Any) {
	if (obj is Person) {
		println(obj.age)
	}
}

//obj에 null이 들어올 수 있다면?
fun printAgeIfPerson(obj: Any?) {
	val person = obj as? Person //person의 타입은 Person?
	println(person?.age)
}
```
# 3가지 특이한 타입

## Any
Any는 Java의 Object와 같은 역할을 한다.
* 모든 Primitive Type의 최상위 타입도 Any이다.
* Any 자체로는 null을 포함할 수 없어, null을 포함하고 싶다면 Any?을 이용해야한다.
* Any에 equals/hashCode/toString이 존재

## Unit
Unit은 Java의 void와 동일한 역할을 한다.
* void와 다르게 Unit은 그 자체로 타입 인자로 사용 가능하다.
* 함수형 프로그래밍에서 Unit은 단 하나의 인스턴스만 갖는 타입을 의미.

## Nothing
함수가 정상적으로 끝나지 않았다는 사실을 표현하는 역할
* 무조건 예외를 반환하는 함수나 무한 루프 함수등에 표현한다.
# String Interpolation, String indexing

### 문자열 가공
Java에서는 StringBuilder를 통해 문자열을 가공하였다. Kotlin에서는 ${}를 통해 문자열 가공이 가능하다.
```kotlin
val hell0 = "안녕하세요"
val person = Person("서범석", 24)
println("이름 : ${person.name}, 나이 : ${person.age}, $hello")
```

### 여러 줄에 걸친 문자열 작성 시
```kotlin
val str = """
	ABC
	$hello
""".trimIndent()

println(str)
```

### 특정 문자 가져오기
인덱스를 통해 가져올 수 있다.
```kotlin
val str = "ABC"
println(str[0])
println(str[1])
```
