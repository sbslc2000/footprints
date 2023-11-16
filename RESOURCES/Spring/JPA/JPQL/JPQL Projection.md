Projection이란 SELECT 절에 조회할 대상을 지정하는 것을 의미한다. 프로젝션의 대상은 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자 등 기본 데이터 타입)등이 될 수 있다.

* `SELECT m FROM Member m` : 엔티티 프로젝션
* `SELECT m.team FROM Member m ` : 엔티티 프로젝션  
	* 이 경우 Join 쿼리가 나감 
* `SELECT m.address FROM Member m`: 임베디드 타입 프로젝션
* `SELECT m.username, m.age FROM Member m` : 스칼라 타입 프로젝션
* DISTINCT로 중복 제거 가능

## 영속성 관리

Entity Projection을 통해 가져온 엔티티는 모두 영속성 컨텍스트로 관리된다. 반면 임베디드 타입 프로젝션이나 스칼라 타입 프로젝션은 단순한 값 타입으로써 가져오게 된다.

## 여러 값 조회
여러 값을 조회할 때에는 여러 방법을 사용할 수 있다.

1. Query 타입으로 조회
```java
List resultList = em.createQuery("select m.username, m.age from Member m");

Object o = resultList.get(0);
Object[] result = (Object[])o;

System.out.println("username = "+result[0]);
System.out.println("age = "+result[1]);
```

2. Object\[] 타입으로 조회
```java
List<Object[]> resultList = em.createQuery("select m.username, m.age from Member m");

Object[] result = resultList.get(0);
System.out.println("username = "+result[0]);
System.out.println("age = "+result[1]);
```

3. new 명령어로 조회

단순 값을 DTO로 바로 조회하는 방식이다. new 키워드와 함께 패키지 명을 포함한 전체 클래스명을 입력해줘야하며, 순서와 타입이 일치하는 생성자를 필요로 한다.

```java
public class MemberDto {
	private String username;
	private int age;
}

...

List<MemberDto> results = em.createQuery("select new com.example.test.MemberDto(m.username, m.age) from Member m");

MemberDto dto = result.get(0);
dto.getUsername();
dto.getAge();
```