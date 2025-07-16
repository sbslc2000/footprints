---
상위 개념: "[[lombok]]"
---
# @SneakyThrows
```java
public class Example
	@SneakyThrows
	public void readFile() {
		Files.readAllBytes(Paths.get("somefile.txt"));
	}
```
`@SneakyThrows`는 lombok이 제공하는 애노테이션으으로, 체크 예외(Checked Exception)을 명시적 처리 없이 던질 수 있게 만들어준다. 롬복은 컴파일 시점에 해당 메서드의 전체를 감싼 뒤 unchecked 예외로 반환하는 코드를 삽입한다.

실제로는 예외가 발생할 수 있음에도 메서드 시그니처에 드러나지 않으므로, 코드의 명시성이 떨어지고 디버깅이 어려워질 수 있다.