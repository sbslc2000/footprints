---
상위 링크: "[[JPQL]]"
---
# JPQL Paging
```java
List<Member> members = em.createQuery("select m from Member m ordery by m.age desc", Member.class)
	.setFirstResult(0)
	.setMaxResults(10)
	.getResultList();
```

위와 같이 작성해주면 각각의 DB에 맞는 페이징 SQL로 번역되어 수행된다. MySQL의 경우 offset, limit, Oracle의 경우 row, rownum으로 번역됨.