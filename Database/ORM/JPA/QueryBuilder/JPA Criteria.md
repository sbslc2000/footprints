---
상위 링크: "[[../JPA]]"
---
# JPA Criteria 
JPA의 동적 쿼리는 만드는 표준 인터페이스이다. 동적인 쿼리를 만들 때에 유용하다.

```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Member> query = cb.createQuery(Member.class);

Root<Member> m = query.from(Member.class);
CriteriaQuery<Member> cq = query.select(m).where(cb.equal(m.get("username"), "kim"));

List<Member> members = em.createQuery(cq).getResultList();
```

JPQL 빌더 역할을 하고 JPA의 공식 기능이지만, 너무 복잡하고 실용성이 없어 QueryDSL을 사용할 것을 권장한다.

