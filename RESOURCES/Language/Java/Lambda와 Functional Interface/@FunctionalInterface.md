---
상위 링크: "[[함수형 인터페이스]]"
---
# @FunctionalInterface
```java
@Documented  
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
public @interface FunctionalInterface {}
```

> An informative annotation type used to indicate that an interface type declaration is intended to be a *functional interface* as defined by the Java Language Specification.

@FunctionalInterface 애노테이션이 붙어진 인터페이스의 구현체는 람다식, 메서드 레퍼런스, 생성자 레퍼런스를 생성하는데에 사용된다.

컴파일러는 해당 애노테이션 타입이 있는 인터페이스에서 다음과 같은 경우에 컴파일에러를 낸다.
1. 타입이 인터페이스가 아니라 애노테이션 타입이거나, enum 타입이거나, 클래스 타입인 경우
2. 해당 타입이 Functional Interface의 요건을 만족하지 못하는 경우

### 왜 RUNTIME Retention을 가지고 있을까?
컴파일레벨에서 Functional Interface의 요건을 만족하는지 검사하는 기능은 SOURCE 정책으로도 충분하다. 그런데 왜 @FunctionalInterface는 RUNTIME Retention을 갖고 있을까?

@FunctionalInterface와 관련한 정보를 런타임에 보존하면 다음과 같은 장점이 생긴다.
1. 런타임에 리플렉션을 통한 분석과 처리를 가능하게 한다.
2. #todo 아래 글 내용을 아직은 잘 모르겠다.
[java - Why does @FunctionalInterface have a RUNTIME retention? - Stack Overflow](https://stackoverflow.com/questions/27121563/why-does-functionalinterface-have-a-runtime-retention)