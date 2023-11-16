# 단항 연산자 / 산술 연산자
Java와 동일하게 사용하면 된다.

# 비교 연산자와 동등성, 동일성

## 비교성
Java와 다르게 객체를 비교할 때 비교 연산자를 쓸 수 있으며, 이 때 compareTo를 자동으로 호출해준다.

```kotlin
val money1 = JavaMoney(2000L)
val money2 = JavaMoney(1000L)

if (money1 > money2) {
	println("Money1이 Money2보다 금액이 큽니다.");
}
```

## 동등성과 동일성
Kotlin에서는 동일성에 === 을 사용하며, 동등성에서는 equals()를 사용하지 않고 대신 \==을 사용한다. 그럼 내부적으로 equals()를 호출해줌.

# 논리 연산자와 코틀린의 특이한 연산자

\||, && 사용은 동일하며, Lazy 연산을 기본적으로 지원한다.


## in, !in
컬렉션이나 범위에 포함되어 있다, 포함되어있지 않다를 의미함

## a..b
a부터 b 까지의 범위 객체를 생성한다.

# 연산자 오버로딩
Kotlin에서는 객체마다 연산자를 직접 정의할 수 있다.

```kotlin
data class Money(
	val amount: Long
) {
	operator fun plus(other: Money): Money {
		return Money(this.amount + other.amount)
	}
}

fun main() {
	val m1 = Money(1000L)
	val m2 = Money(2000L)

	println(m1 + m2) //plus 호출
}
```