---
상위 개념: "[[Method Area]]"
---
# Runtime Constant Pool
런타임 상수 풀(Runtime Constant Pool)은 JVM에서 클래스를 로딩할 때 생성되는 상수 값과 심볼릭 참조를 저장하는 특별한 메모리 영역이다. 이는 JVM의 메서드 영역 안에 포함되어 있으며, 각 클래스(또는 인터페이스)마다 고유하게 존재한다.

## Literal Constant
```java
public class SomeClass {
	public static final String CONST1 = "Hello";
	public static final int CONST2 = 100;

	public void someMethod() {
		System.out.println("Hello, World!")
	}
}
```
`final static`이 붙은 리터럴 상수는 클래스 로딩 시점에 런타임 상수 풀에 로딩된다. 또한 위 6번 라인의 "Hello, World!"도 문자열 리터럴로 취급되며 상수 풀에 로딩된다. 

## Symbolic Reference
심볼릭 참조(Symbolic Reference)는 어떤 대상을 직접적인 메모리 주소나 포인터로 가리키느게 아니라, 문자열 기반의 '이름'으로 참조하는 방식이다.

자바의 클래스 파일(.class)은 처음부터 직접적인 메모리 주소를 알 수 없기 때문에, 대신 심볼 이름을 참조해두고, 클래스 로딩이 된 이후 실제 메모리 주소를 바인딩한다.

```java
public class A {
	public void method() {
		B b = new B();
	}
}
```

클래스 A를 메서드 영역에 로딩할 때, A 내부에 new B()와 같은 코드가 존재하더라도 B 클래스의 실제 메모리 주소나 바이트코드 위치는 아직 알 수 없다. 이는 **컴파일 시점에도 마찬가지**로, 자바 컴파일러는 B 클래스의 정의를 심볼 이름(문자열)로만 인식하며, 직접 참조가 아닌 ‘심볼릭 참조’로 .class 파일의 상수 풀(Constant Pool)에 기록한다.

이 심볼릭 참조는 클래스 A가 로딩되면 JVM의 메서드 영역의 런타임 상수 풀에 포함되어 저장된다. 이후 new B() 명령이 실제로 실행되는 시점(즉, 바이트코드가 수행되는 시점)에 JVM은 해당 심볼릭 참조를 바탕으로 B 클래스를 메모리에서 찾거나, 아직 로딩되지 않았다면 클래스로더를 통해 로딩한다.