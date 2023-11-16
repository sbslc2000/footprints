IDENTITY 전략에서 JPA는 PK의 생성과 설정을 DBMS에 위임합니다.
IDENTIY 전략 하에서 Entity의 ID는 insert query가 발생하는 시점에 알 수 있습니다. em.persist()로 영속화 되기 위해서는 Entity 객체에 ID가 존재해야하기 때문에, 쿼리 지연 저장소를 사용하는 JPA 구조에서는 문제가 발생할 수 있습니다. 
JPA는 이러한 문제를 insert query를 먼저 전송하는 방식으로 해결합니다. em.persist() 가 발생한다면 즉시 insert query를 실행하고 DB에서 식별자를 조회한 뒤, 해당 값을 id에 할당하고 엔티티를 영속화합니다. 이는 IDENTITY 전략에서만 적용되는 방법으로, 다른 전략에서는 insert query가 즉시 실행되지 않습니다.
```java
@Entity
public class Member {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
```

#genratedValue