---
상위 링크: "[[JPQL]]"
---
# JPQL Named Query

Named Query는 정적인 JPQL을 사용하기 위한 방법이다.

정적 쿼리이기 때문에 런타임에 수정될 수 없고 유연하지 않으나, 애플리케이션 로딩 시점에 초기화하므로 여러 이점을 가질 수 있다.

* 쿼리 검증
	* 쿼리가 잘못되었다면 초기화 시점에 에러를 발생시킨다.
* 번역 캐싱
	* SQL로 번역된 결과를 캐싱하여 추가적인 코스트 없이 쿼리를 보낼 수 있다.

# 예시

예시 1. 애노테이션을 통해 사용하는 Named Query

```java
@Entity
@NamedQuery(
	name = "Member.findByUsername",
	query="select m from Member m where m.username = :username")
public class Member {
...
}

List<Member> resultList =
em.createNamedQuery("Member.findByUsername", Member.class)
	.setParameter("username", "회원1")
	.getResultList();
```

예시 2. XML에 정의하는 Named Query
![](https://i.imgur.com/AVa6sjo.png)

Named Query는 XML이 항상 우선권을 가진다. 또한 XML을 사용하면 운영 환경에 따라 다른 쿼리를 발생시키기에 용이하다.

> 스프링 데이터 JPA에서 @Query 에 넣는 쿼리가 Named Query이다!