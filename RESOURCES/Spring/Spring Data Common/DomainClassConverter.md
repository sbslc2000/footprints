---
상위 개념: "[[웹 관련 기능]]"
---
# 도메인 클래스 컨버터
```java
public class DomainClassConverter<T extends ConversionService & ConverterRegistry>  
       implements ConditionalGenericConverter, ApplicationContextAware { ...}
```
내부적으로는 크게 2가지 컨버터가 등록이 된다.
* ToEntityConverter : ID를 받아서 Entity 타입으로 변환하는 컨버터
* ToIdConverter : Entity 타입으로부터 ID를 가져오는 컨버터


## 예제
```java
@GetMapping("/posts/{id}")
public Post getPost(@PathVariable("id") Long id) {
	Post post = postRepository.findById(id).get();
	return post;
}
```
DomainClassConverter를 사용하면 PathVariable 등의 바인딩 애노테이션들을 사용하여 엔티티를 바로 받을 수 있다. 이 때 더 이상 변수명과 variable 명이 같지 않으므로 변수명을 필수로 입력해주어야 한다.
```java
@GetMapping("/posts/{id}")
public Post getPost(@PathVariable("id") Post post) {
	return post;
}
```

이렇게 가져온 Post는 OSIV이 true인 경우에만 영속성 컨텍스트에 포함되고 나머지 경우는 detached 상태로 받아진다. 따라서 이를 추후 서비스 레벨에서 사용할 때에 유의해야한다.