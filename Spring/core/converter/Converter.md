---
상위 개념: "[[Spring Type Converting]]"
"":
---
# Converter
```java
package org.springframework.core.convert.converter;

public interface Converter<S, T> {
	T convert(S source);
}
```

스프링은 확장가능한 컨버터 인터페이스를 제공한다. 스프링 내부 동작에 사용자가 정의한 타입으로의 변환이 필요하다면 이 컨버터 인터페이스를 구현해서 등록하면 된다.