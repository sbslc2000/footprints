# try catch finally 구문
```kotlin
fun parseIntOrThrow(str: String): Int {
	try {
		return str.toInt();
	} catch (e : NumberFormatException) {
		throw IllegalArgumentException("주어진 ${str}은 숫자가 아닙니다.")
	}
}
```

```kotlin
//실패시 null을 반환하는 예제
fun parseIntOrThrowV2(str: String): Int? {
	return try {
		str.toInt()
	} catch (e: NumberFormatException) {
		null
	}
}
```
try-catch 구문도 expressions으로 취급되어 return을 한 번만 쓸 수 있다.
# Checked Exception과 Unchecked Exception

Kotlin에서는 Checked Exception과 Unchecked Exception을 구분하지 않고, 모두 Unchecked Exception으로 간주한다.

```kotlin
fun readFile() {
	val currentFile = File(".")
	val file = File(currentFile.absolutePath + "/a.txt")
	val reader = BufferedReader(FileReader(file))
	println(reader.readLine())
	reader.close()
}
```
# Try with resources

try with resources 문법은 사라지고, kotlin의 use를 사용하여 대체할 수 있다.
```kotlin
fun readFile(path: String) {
	BufferedReader(FileReader(path)).use { reader ->
		println(reader.readLine())
	}
}
```