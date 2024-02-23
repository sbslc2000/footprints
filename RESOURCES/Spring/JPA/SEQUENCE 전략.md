
데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트 (예: 오라클 시퀀스)
```java
@Entity
@SequenceGenerator(
	name = “MEMBER_SEQ_GENERATOR",
	sequenceName = “MEMBER_SEQ", //매핑할 데이터베이스 시퀀스 이름
	initialValue = 1, allocationSize = 1)
public class Member {
	@Id
	@GeneratedValue(strategy = GenerationType.SEQUENCE)
	private Long id;
```
![](https://i.imgur.com/AASCZHl.png)

Sequence 전략에서는 [[IDENTITY 전략]]과 다르게 INSERT Query가 발생하기 전에 다음 값을 알 수 있어서, 이를 쿼리를 통해 얻어서 영속성 컨텍스트에 넣어준다. 따라서 INSERT Query가 발생하는 시점을 트랜잭션 시점으로 미룰 수 있다.
### AllocationSize
allocationSize 속성을 통해 성능 최적화를 할 수 있다.
SEQUENCE 전략에서는 엔티티가 생성될 때마다 next pk value를 네트워크를 통해 DB에서 가져와야하는데, 이 경우 빈번한 DB 접속에 의한 오버헤드가 발생하여 성능 저하의 원인이 된다. 이러한 문제를 JPA에서는 allocationSize로 해결한다.
AllocationSize를 통해 DB의 시퀀스가 특정한 수 만큼 증가하게 만들며, 그 사이에 해당하는 값들은 애플리케이션 레벨에서 관리한다. 기본 값은 50이므로, 애플리케이션에서는 엔티티가 50번 생성될 때마다 DB Sequence를 올릴것을 호출할 것이다.

> [!NOTE] 이러한 구조는 동기화 문제가 발생할 것 같은데?
> 서버가 여러개라면 각각의 AP 서버가 DB 시퀀스 호출을 통해 얻은 ID range를 각각 관리하게 된다. 예를 들어 AP 서버 1은 1~50의 번호를 할당받고, AP 서버 2는 이후 51~100의 번호를 할당받는다. 이 경우 각각의 AP 서버가 각각의 번호에서만 할당하면 동기화 문제는 발생하지 않는다.
