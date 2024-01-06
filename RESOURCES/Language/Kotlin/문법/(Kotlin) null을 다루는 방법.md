---
상위 개념: "[[Kotlin 문법]]"
---
## null을 다루는 방법
Kotlin은 null이 가능한 타입을 완전히 다르게 취급한다.

```kotlin

public boolean startsWithA1(String str) {
	if(str == null) {
		throw new IllegalArgumentException("null이 들어왔습니다.");
	}
	return str.startsWith("A");
}

fun startsWithA1(str: String?) : Boolean {
	return str?.startsWith("A") ?: throw IllegalArgumentException("...")
	//null이 아닌 경우에는 startsWith의 호출 결과를 반환하되, 만약 null인 경우에는 exception을 던져라
}
```

```kotlin
public Boolean startsWithA2(String str) {
	if (str == null)
		return null;
	return str.startsWit("A");
}

fun startsWithA2(str: String?) : Boolean? {
	return str?.startsWith("A")
}
```

```kotlin
public boolean startsWithA3(String str) {
	if(str == null)
		return false;
	return str.startsWith("A");
}

fun startsWithA3(str: String?) : Boolean {
	//str.startsWith("A") //수행 불가능, null 체크가 되지 않는다면 바로 명령어를 수행할 수 없음
	return str?.startsWith("A") ?: false
}
```

### Safe Call과 Elvis 연산자

Safe Call
null인 경우 null을 반환, null이 아닌 경우 호출
```kotlin
val str: String? = "ABC"
str.length //불가능
str?.length // 가능 , null이 아닐때만 호출, null이라면 null을 반환
```

Elvis 연산자
앞의 연산 결과가 null인 경우에 뒤의 식을 호출한다.
```kotlin
val str: String ? = "ABC"
str?.length ?: 0
//앞의 결과가 null이라면 (str이 null인 경우) 0을 반환한다.
//앞의 결과가 null이 아니라면(str이 null이 아닌 경우) str.length를 반환
```

early return 에서도 Elvis 연산자를 사용할 수 있다.
```kotlin
fun calculate(number: Long?): Long {
	number ?: return 0	
}
```

### null 아님 단언
nullable type이지만, 아무리 생각해도 null이 될 수 없는 경우.
위험하니 조심히 사용해야할 듯
```kotlin
fun startsWith(str: String?) : Boolean {
	return str!!.startsWith("A")
}
```

## Kotlin에서 Java 코드 가져와 사용하기
### Platform Type

```kotlin

public class Person {

	private final String name;
	@Nullable //이 경우 String? 로 취급
	@NotNull //이 경우 String으로 취급
	//이런 플랫폼 타입 애노테이션이 없다면? 컴파일단에서 안잡히고 런타임 에러가 발생
	public String getName() {
		return name;
	}
}
fun main() {
	val person = Person("서범석")

	startsWithA(person.name) 
}

fun startsWithA(str: String): Boolean {
	return str.startsWith("A")
}
```