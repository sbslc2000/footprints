---
상위 개념: "[[Kotlin 문법]]"
---
# for-each
: 대신 in 을 사용한다. in 뒤에는 iterable이 구현된 타입이라면 모두 들어갈 수 있다.
```kotlin
val numbers = listOf(1L, 2L, 3L)
for (number in numbers) {
	println(number)
}
```

# 전통적인 for문

```kotlin
//정방향
for (i in 1..3) {
	println(i)
}

//역방향
for (i in 3 downto 1) {
	println(i)
}

//2칸씩
for (i in 1..5 step 2) {
	println(i)
}
```

# Progression

.. 연산자는 범위를 만들어내는 연산자이다. 범위에 해당하는 Range라는 클래스가 있으며,이는 IntProgression을 상속받고 있다.

-> 1..3 => 1부터 시작해서 3으로 끝나는 등차수열을 만들어줘 (기본 공차는 1)
--> 공차는 step 과 downto 로 지정할 수 있음!
---> step 과 downto는 함수이다! (중위함수) 
-----> 중위함수는 "변수 함수이름 argument" 형태로 함수를 호출

# while
java와 동일하다.