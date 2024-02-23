---
상위 개념: "[[Spring Data COMMON]]"
---
# Repository

![](https://i.imgur.com/kuIu6sB.png)

Spring Data COMMON의 리포지토리는 위와 같은 계층구조를 갖는다.

가장 상위의 Repository 인터페이스는 실질적인 기능을 하지는 않고, 이것을 상속한 클래스가 레포지토리 역할을 한다는 의미만 제공한다. 이러한 형태로 인터페이스를 사용하는 것을 Marker Interface라고도 부른다.

CrudRepository와 그 하위의 인터페이스들은 @NoRepositoryBean이라는 애노테이션이 부착되어있다. @NoRepositoryBean 애노테이션은 해당 클래스의 빈이 사용자에 의해서 직접 등록되지 않도록 막아주는 역할을 한다.

Repository 인터페이스와 다르게 CrudRepository부터는 기능을 제공한다. `save`, `saveAll`, `findById`, `existsById`, `findAll`, `findAllById`, `count`, `deleteById`, `delete` 와 같은 CRUD 기능을 제공한다.

PagingAndSortingRepository에서는 findAll의 파라미터로 Pageable의 구현체와 Sort를 담아 전송할 수 있다.

#todo <!-- 아래 문장은 추후 Pageable을 설명하는 문서로 이관 예정 --> 
Page 객체의 size 속성은 요청 시 한 페이지에 담을 개수를 의미하며, numberOfElements는 실제로 해당 페이지에 들어온 요소의 개수를 의미한다.


# 활용법

## @RepositoryDefinition
JpaRepository를 사용할 때에 너무 많은 기능이 상속되어 이를 제한하고 싶은 경우 @RepositoryDefinition 애노테이션을 사용할 수 있다.
```java

//RepositoryDefinition을 통해 저장할 도메인 클래스와 id 타입을 지정
@RepositoryDefinition(
	domainClass = Member.class,
	idClass = Long.class
)
public interface CustomMemberRepository {  
  //이곳에 작성된 메서드는 Spring Data가 구현할 수 있다면 구현한 뒤 빈으로 등록
	Member save(Member member);

	List<Member> findAll();
}
```

이 때 JPA가 예측할 수 없는 메서드라면 빈 생성 시점에 에러가 발생한다. 아마 Spring Data JPA를 의존하고 있다면 Spring Data JPA의 기능을 활용하여 메서드를 만드는 것 같다.

이 기능은 Spring Data JPA에 의존적인 것이 아니어서 Spring Data JDBC나 기타 다른 저장방식을 사용할 때에도 활용할 수 있다. 빈을 등록하는 방식은 스프링부트의 자동 구성에 의해 활성화된 모듈의 방식을 채택하는 것으로 보인다.

## Repository 인터페이스를 사용한 중복 메서드 처리
위와 같은 방식을 사용할 때에 인터페이스마다 중복된 메서드들이 존재할 수 있다. 이를 상속관계로 해결하기 위해서 Spring Data 의 리포지토리 중 가장 상위에 있는 Repository 인터페이스를 활용할 수 있다.
```java
@NoRepositoryBean  
public interface MyRepository<T, ID extends Serializable> extends Repository<T, ID> {  
  
        <E extends T> E save(E entity);  
  
        <E extends T> List<E> findAll();  
}
```

* ID가 Serializable해야하는 이유는?
* **@NoRepositoryBean을 붙여야하는 이유는?**
	* 해당 인터페이스가 리포지토리 빈으로 등록되는 것을 막아준다. 만약 PagingAndSortingRepository 인터페이스에 @NoRepositoryBean 애노테이션이 사용되지 않았다면, 스프링 데이터는 해당 인터페이스를 대상으로 레포지토리 빈을 생성하여 등록할 것이다.
* **제너릭에 extends를 했을 때의 효과**
	* 하나의 인터페이스로 Member과 이를 상속한 Customer까지 처리 가능해진다.

```java
public interface RepositoryDefinitionMemberRepository extends MyRepository<Member,Long> {  
}
```
위와 같이 정의한 인터페이스를 상속한 인터페이스를 만들어 놓으면 빈이 생성된다.