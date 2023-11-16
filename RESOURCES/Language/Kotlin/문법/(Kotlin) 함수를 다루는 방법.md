# 함수 선언 문법
```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```

* public 은 생략 가능하다
* if-else 는 expression이므로 return을 빼낼 수 있다.
* 함수가 하나의 결과 값이라면, block 대신 =을 사용하고 return을 빼 줄 수 있다.
* 반환 타입 추론 가능시 반환 타입 생략 가능
* =을 사용할 시 한 줄로 사용 가능하며, {} 도 생략할 수 있다.
* 클래스 내부에도, 파일 최상단에도 존재할 수 있다.
# default parameter
![](https://i.imgur.com/MfcWF1o.png)
위와 같은 경우 useNewLine이 반복되는 경우 오버로딩을 통해 사용 측에서 작성을 간단하게 할 수 있다. 이 문제를 코틀린은 default parameter로 해결한다.

```kotlin
fun main() {
	repeat("Hello, World!")
}

fun repeat(
	str: String,
	num: Int = 3,
	useNewLine: Boolean = true
) {
	for (i in 1..num) {
		if (useNewLine) {
			println(str)
		} else {
			print(str)
		}
	}
}
```


# named argument (parameter)

```kotlin
fun main() {
	repeat("Hello, world!", 3,  false)
	//만약 3에 대한 것을 기본값으로 하고 싶다면?
	repeat("Hello, World!", useNewLine = false)

}
```
파라미터를 지정해줄 수 있다.
** 주의 : Kotlin에서 Java의 함수를 가져다 쓸 때에는 named argument를 사용할 수 없다.

# 같은 타입의 여러 파라미터 받기 (가변인자)

![](https://i.imgur.com/mDwI7Y6.png)

```kotlin
fun main() {
	printAll("A","B","C")

	val array = arrayOf("A", "B", "C")
	printAll(*array)
}

fun printAll(vararg strings: String) {
	for(str in strings) 
		println(str)
}
```

* vararg을 사용한다.
* 사용하기 위해서는 spread 연산자(\*)를 사용한다. 