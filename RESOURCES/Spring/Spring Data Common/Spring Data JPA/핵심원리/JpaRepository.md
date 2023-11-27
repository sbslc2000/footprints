# JpaRepository
```java
public interface MemberRepository extends JpaRepository<Member,Long> {
}
```
JpaRepository는 Spring Data JPA에서 제공하는 인터페이스로, Spring Data Common이 제공하는 PagingAndSortingRepository를 상속한다. 이를 통해 코드 한 줄 없이도 DAO 역할을 하는 빈을 등록할 수 있다.

SpringData JpaRepository를 사용하려면 @EnableJpaRepositories를 사용해야하지만, 스프링부트는 이를 자동 설정해준다. 부트를 사용하지 않을 때는 @Configuration과 함께 사용해야한다.

JpaRepository 를 상속한 인터페이스는 @Repository 애노테이션을 붙일 필요가 없는데, 이는 JpaRepository의 구현체가 되는 SimpleJpaRepository에는 @Repository 애노테이션이 이미 붙어있기 때문이다. 빈 등록의 작동 원리는 [여기](JpaRepository%20빈%20등록)로

## save 메서드
save는 2가지 용도로 사용할 수 있다.
1. Transient 상태 객체의 persist
2. Detached 상태 객체의 merge

Transient 상태인지 Detached 상태인지 판단하는 기준은 @Id 프로퍼티이다. 이 값이 null이면 transient, null이 아닌 경우 detached로 판단한다.
```java
@Transactional  
@Override  
public <S extends T> S save(S entity) {  
  
    Assert.notNull(entity, "Entity must not be null");  
  
    if (entityInformation.isNew(entity)) {  
       entityManager.persist(entity);  
       return entity;  
    } else {  
       return entityManager.merge(entity);  
    }  
}
```

save를 통해 반환되는 엔티티는 무조건 영속화 상태의 객체이다. 반면 파라미터로 전달된 post가 영속된 상태인지는 persist, merge 여부에 따라 달라진다.
```java
@Rollback(false)  
@Test  
void persistTest() {  
    Post post = new Post();  
    post.setTitle("hello");  
  
    em.persist(post);  
  
    assertThat(em.contains(post)).isTrue();  
    assertThat(post.getId()).isNotNull();  
}  
  
@Rollback(false)  
@Test  
void mergeTest() {  
    Post post = new Post();  
    post.setTitle("hello");  
  
    Post merge = em.merge(post);  
  
    assertThat(em.contains(post)).isFalse();  
    assertThat(em.contains(merge)).isTrue();  
    assertThat(merge).isNotNull();  
    assertThat(merge.getId()).isEqualTo(1L);  
}
```

## 쿼리 메서드

### NamedQuery
엔티티에 메타정보로 제공되는 namedquery를 JpaRepository에 선언하여 사용할 수 있다.
```java
@NamedQuery(name = "Post.findByTitle", query = "SELECT p FROM Post AS p WHERE p.title = :title")  
public class Post extends AbstractAggregateRoot<Post>{
```

```java
public interface PostRepository extends JpaRepository<Post,Long> {

	...
	List<Post> findByTitle(String title);
}
```

엔티티에 부가정보를 제공하는 것이 싫다면 Repository 인터페이스에 @Query 애노테이션을 통해 제공할 수 있다.

```java
public interface PostRepository extends JpaRepository<Post,Long> {

	...
	@NamedQuery("select p from Post As p where p.title = :title")
	List<Post> findByTitle(String title);
}
```

## Sort
```java
@Query("select p from Post as p where p.title = ?1")
List<Post> findByTitle(String title, Sort sort);
```

Sort는 엔티티의 프로퍼티 혹은 alias가 없는 경우 예외를 발생시킨다.
```java
postRepository.findByTitle("title", Sort.by("LENGTH(title)"));
```

이것을 우회하는 방법으로는 JpaSort.unsafe()를 사용할 수 있다.
```java
postRepository.findByTitle("title", JpaSort.unsafe("LENGTH(title)"));
```

## Named Parameter와 SPEL
쿼리 파라미터에 바인딩 하는 방법은 2개가 있다.
1. 순서를 통해 바인딩
2. Named Parameter (:title)을 통한 바인딩

### SPEL
SPEL을 사용하여 특정 엔티티 이름에 종속되지 않은 쿼리를 만들어 재사용성을 늘릴 수 있다. \#entityName에는 해당 레포지토리의 엔티티 이름이 바인딩된다.
```java
@Query("select p from #{#entityName} as p WHERE p.title = :title")
List<Post> findByTitle(String title);
```

## Update 쿼리
@Modifying 애노테이션 등을 사용하여 데이터를 조작하는 쿼리를 작성할 수 있다.
```java
@Modifying
@Query("update ...")
void updateTitle(Post post);
```
**추천하지 않는다.** update의 DB 조작 결과는 영속성 캐시에 반영되지 않는다. 이를 해결하기 위해서는 em.flush(), em.clear() 등이 수행되어야 할 것이다. 이러한 속성은 @Modifying 애노테이션의 속성으로 지정해줄 수 있다. 

## EntityGraph
EntityGraph는 FetchMode를 유연하게 설정할 수 있는 기능을 제공한다.

* **@NamedEntityGraph**
	* @Entity에서 재사용할 여러 엔티티 그룹을 정의할 때 사용
* **@EntityGraph**
	* @NamedEntityGraph에 정의되어있는 엔티티 그룹을 사용
	* 그래프 타입 설정 가능
		* (기본값) FETCH: 설정한 엔티티 프로퍼티는 EAGER 적용, 나머지는 LAZY
		* LOAD : 설정한 엔티티 프로퍼티는 EAGER 적용, 나머지는 기본 페치 전략 따름

```java
@NamedEntityGraph(name = "Comment.post",
	attributeNodes = @NamedAttributeNode("post"))
@Entity
class Comment {
...
}
```

```java
public interface CommentRepository extends JpaRepository<Comment,Long> {

	@EntityGraph(value = "Comment.post") //"post는 EAGER 나머지는 LAZY"
	Optional<Comment> getById(Long id);
}
```
메서드마다 각각의 페칭 전략을 가질 수 있는 기능을 지원한다.

아래처럼 NamedEntityGraph를 엔티티 레벨에 생성하지 않고 쿼리 메서드에 정의할 수 있다. 재사용은 불가능하다.
```java
@EntityGraph(attributePaths = "post")
Optional<Comment> getById(Long id);
```

#todo <!--Spring 애노테이션의 @Repository로 이전 -->
스프링은 @Repository가 붙은 오브젝트에서 발생한 예외(SQLException, JPA관련 예외)를DataAccessException으로 변환해준다. 과거의 SQLException은 너무 많은 예외들의 내용을 하나의 Exception클래스에 담고 메시지를 통해 설명이 이루어졌기 때문에, 스프링은 예외를 분석하여 구체적인 예외를 되던져 준다. 하지만 최근에 Hibernate등의 기술들은 충분히 설명되어있는 exception을 반환하기도 한다.

## Projection
일부분에 해당하는 데이터만 select 할 수 있는 기술
인터페이스 기반과 클래스 기반으로 나눌 수 있으며, 이들은 각각 Open 프로젝션 방식과 Closed 프로젝션 방식이 있다. **Open Projection**은 일단 다 가져온 이후 보고싶은 것만 필터링하는 것이며, **Closed Projection**은 최초에 가져올 때부터 필요한 정보만 가져오는 것이다.
### 인터페이스 기반 프로젝션
* closed projection
아래는 관심있는 필드의 정보만을 가져오는 인터페이스이다.
```java
public interface CommentSummary {
	String getComment();
	int getUp();
	int getDown();
}
```

```java
public interface CommentRepository extends JpaRepository<Comment,Long> {
	...
	List<CommentSummary> findByPost_id(Long id);
}
```
이 경우 SQL도 필요한 컬럼만을 가져오도록 변경된다.

* open projection
아래처럼 사용하면 일단 target에 대한 정보를 가져와야하기 때문에 모든 컬럼을 가져오게 된다.
```java
public interface CommentSummary {
	String getComment();
	int getUp();
	int getDown();

	@Value("#{target.up ' ' + target.down}")
	String getVotes();
}
```

위와 같이 조합된 컬럼을 사용하면서 closed projection을 하기 위해서는 다음과 같이 하면 된다.
```java
public interface CommentSummary {
	String getComment();
	int getUp();
	int getDown();

	String getVotes() {
		return getUp() + " " + getDown();
	}
}
```

### 클래스 기반 프로젝션

* closed projection
```java
public class CommentSummary {
	private String comment;
	private int up;
	private int down;

	public CommentSummary(String comment, int up, int down) {
		this.comment = comment;
		this.up = up;
		this.down = down;
	}
	
	public String getComment();
	public int getUp();
	public int getDown();

	String getVotes() {
		return getUp() + " " + getDown();
	}
}
```

이렇게 하면 클래스를 사용하면서 closed projection을 사용할 수 있다. 하지만 이 방식은 인터페이스에 비하여 많은 코드가 들어가는 반면 효과는 동일하므로 인터페이스를 쓰는 것이 더 나을지도 모르겠다.

### 여러개의 타입을 처리해야한다면
만약 여러개의 클래스(혹은 인터페이스)를 처리해야할 때 어떻게 하면 좋을까?
```java
public interface CommentRepository extends JpaRepository<Comment,Long> {
	...
	List<CommentSummary> findByPost_id(Long id);
	List<CommentOnly> findByPost_id(Long id); //compile error
}
```

이때는 Generic을 사용하여 유연하게 처리할 수 있다. -> **Dynamic Projection**
```java
public interface CommentRepository extends JpaRepository<Comment,Long> {
	...
	//대신 파라미터로 타입을 넘겨야한다.
	<T> List<T> findByPost_id(Long id, Class<T> type);
}
```

## Specification
Specification은 DDD 책에서 언급되는 개념으로 Querydsl의 Predicate와 유사하다.

### 설정하는 방법
* jpa-model-generator 추가

어우 구버전

## Query By Example
QBE는 필드 이름을 작성할 필요 없이 단순한 인터페이스를 통해 동적으로 쿼리를 만드는 기능을 제공하는 사용자 친화적인 쿼리 기술입니다.

```java
Comment prove = new Comment();
prove.setBest(true);

//기본적으로는 모든 필드를 비교하여 가져오지만, 여러 메서드를 통해 적용될 프로퍼티를 한정할 수 있다.
ExampleMatcher.matching()
	.withIncludeNullValues()
	.withMatcher("best", isTrue());

```

단점
* 프로퍼티 그룹에 대한 제약조건을 만들지 못한다.
* 범위 값 조회를 하지 못한다.

## Auditing
엔티티의 변화가 나타났을 때, 이것이 언제 누구로부터 변경됐는지 기록하는 방법

```java
public class Comment {

	@CreatedAt
	private Date createdAt;

	@CreatedBy
	@ManyToOne
	private Account createdBy;

	@LastModifiedDate
	private Date createdAt;

	@LastModifiedBy
	@ManyToOne
	private Account createdBy;
}
```

Auditing을 사용하기 위해서는 애노테이션을 작성해주어야 한다.

```java
@SpringBootApplication
@EnableJpaAuditing
public class Application {
...
}
```

Entity에는 AuditingListener를 설정해주어야 한다.
```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Comment { ... }
```

이제 Date는 알아서 갱신된다. 하지만 Account는 어떻게 업데이트시켜야 할까?
Spring Security를 사용하면 AuditAware라는 인터페이스를 구현하여 수정,생성 시의 사용자를 반환하게 만들 수 있다.
```java
public class AccountAuditAware implements AuditorAware<Account> {
	@Override
	public Optional<Account> getCurrentAuditor() {
		Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

		return (MyUserDetails) authentication.getPrincipal().getUser();
	}
}
```

이 경우 등록된 빈의 이름을 설정에서 제공해주어야 한다.
```java
@SpringBootApplication
@EnableJpaAuditing(autidotorAwareRef = "accountAuditAware")
```


### Jpa의 Lifecycle Event를 사용하는 방법
Jpa 엔티티의 lifecycle 간에는 이벤트가 발생하여 콜백 메서드를 수행시킬 수 있다.

```java
@Entity 
class Post {

	@PrePersist
	public void prePersist() {
		this.createdAt = new Date();
		this.createdBy = //ThreadLocal을 통해 유저 가져오기
	}
}
```

@PreUpdate, @PrePersist 등을 사용하여 생성시, 수정시 필요한 값들을 넣을 수 있다. 