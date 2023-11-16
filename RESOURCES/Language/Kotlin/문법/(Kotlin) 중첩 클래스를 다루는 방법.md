# 중첩 클래스의 종류

Java에서의 중첩 클래스
- Static 을 사용하는 중첩 클래스
- Static을 사용하지 않는 중첩 클래스
	- Inner class
	- Local class
	- Anonymous class

# Kotlin에서의 중첩 클래스

```kotlin
class House(
	private val address: String,
	private val livingRoom: LivingRoom
) {
	class LivingRoom(
		private val area: Double
	)
}
```