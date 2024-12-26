---
상위 링크: "[[../Spring|Spring]]"
공식 링크: https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/cache/annotation/Cacheable.html
---
# @Cacheable 
```java
@Override  
@Cacheable("books")  
public Book getByIsbn(String isbn) {  
    simulateSlowService();  
    return new Book(isbn, "Some book");  
}
```
@Cacheable 애노테이션은 org.springframework.cache에서 제공하는 애노테이션으로, 해당 메서드가 캐싱 가능함을 뜻한다.

캐시를 동작시키기 위해서는 @Cacheable 애노테이션을 처리하는 기능을 활성화하기 위해 @EnableCaching과 함께 사용해야 한다. @EnableCaching이 설정된다면, 스프링 빈 구성 이후 각각의 빈에 @Cacheable이 있는지 확인하고, 있다면 동적 프록시를 생성하여 캐싱 동작을 수행하게 만든다.