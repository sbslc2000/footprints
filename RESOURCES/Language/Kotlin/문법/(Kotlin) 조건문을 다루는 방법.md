# If 문
java와 동일함
```kotlin
fun validateScoreIsNotNegative(score: Int) {
	if(score < 0)
		throw IllegalArgumentException("${score}는 0보다 작을 수 없습니다.")
}
```

```kotlin
fun getPassOrFail(score: Int) : String {
	return if (score >= 50) {
		 "P" 
	} else {
		"F"
	}
}
```

Java에서 if-else는 Statement이지만, Kotlin에서 if-else는 Expression이다.

> [!info]
> * Statement: 프로그램의 문장, 하나의 값으로 도출되지 않는다
> * Expression: 하나의 값으로 도출되는 문장
> * Statement가 Expression을 포함한다. 

따라서 Kotlin에서는 if - else 를 통째로 하나의 값으로 도출해낼 수 있으므로 위와 같이 동작할 수 있다.

```kotlin
fun getGrade(score: Int): String {
	return 
	if(score >= 90) {"A"}
	else if (score >= 80) {"B"}
	...
	else {"F"}
}
```


# Expression & Statement

어떠한 값이 특정 범위에 포함되어 있는지, 포함되어 있지 않은지
```kotlin
//score의 범위가 0 부터 100 사이에 있지 않다면
fun validateScoreIsNotNegative(score: Int) {
	if(score !in 0..100)
		throw IllegalArgumentException("...")
}

//score의 범위가 0 부터 100 사이에 있다면
fun validateScoreIsNotNegative(score: Int) {
	if(score in 0..100)
		throw IllegalArgumentException("...")
}

```


# switch와 when

java의 switch는 kotlin에서 when으로 사용할 수 있다.
```text
when (값) {
	조건부 -> 어떠한 구문
	조건부 -> 어떠한 구문
	else -> 어떠한 구문
}
```
* 조건부에는 어떠한 expression이라도 들어갈 수 있다. (ex. is Type)
* 여러개의 조건을 동시에 검사할 수도 있다.

```kotlin
fun getGradeWithSwitch(score: Int) : String {
	return when (score / 10) {
		9 -> "A"
		8 -> "B"
		7 -> "C"
		else -> "D"
	}
}

//다음과 같이 사용 가능
fun getGradeWithSwitch(score: Int) : String {
	return when (score) {
		90..99 -> "A"
		80..89 -> "B"
		70..79 -> "C"
		else -> "D"
	}
}
```

```kotlin
//조건부에는 어떠한 expression이라도 들어갈 수 있다. (ex. is Type)
fun startsWithA(obj: Any) : Boolean {
	return when (obj) {
		is String -> obj.startsWith("A") // Smart Cast
		else -> false
	}
}

//여러개의 조건을 동시에 검사할 수도 있다.
fun judgeNumber(number: Int) {
	when(number) {
		1, 0, -1 -> println("something")
		else -> println("1, 0, -1이 아닙니다.")
	}
}

fun judgeNumber2(number: Int) {
	when {
		number == 0 -> println("주어진 숫자는 0입니다.")
		number % 2 -> println("주어진 숫자는 짝수입니다.")
		else -> println("주어진 숫자는 홀수입니다.")
	}

}
```