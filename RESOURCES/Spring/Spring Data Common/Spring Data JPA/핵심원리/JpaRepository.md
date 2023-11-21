# JpaRepository
```java
public interface MemberRepository extends JpaRepository<Member,Long> {
}
```
JpaRepository는 Spring Data JPA에서 제공하는 인터페이스로, Spring Data Common이 제공하는 PagingAndSortingRepository를 상속한다. 이를 통해 코드 한 줄 없이도 DAO 역할을 하는 빈을 등록할 수 있다.
