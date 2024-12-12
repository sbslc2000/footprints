일반적으로 JPA가 제공하는 [변경 감지](Persistence%20Context.md##변경%20감지*Dirty%20Checking*)는 각각의 엔티티 마다 쿼리를 발생시킨다. 이러한 문제를 해결하기 위해 JPA는 Bulk 연산 API를 제공한다.


# 사용 방법
```java

int resultCount = em.createQuery("update Member m set m.age = 20")
	.executeUpdate();

//resultCount: 업데이트 된 row 개수
```

# 주의 사항

벌크 연산은 **영속성 컨텍스트를 무시하고** 데이터베이스에 직접 쿼리를 보낸다. 따라서 다음 방식을 고려해야한다. (벌크 연산의 결과는 영속성 컨텍스트의 엔티티에 반영되지 않는다.)

1. 벌크 연산을 먼저 실행한다. 이러면 영속성 컨텍스트에 애초에 뭐가 존재하지 않으니 초기화에서 문제가 발생하지 않음
2. 벌크 연산 수행 후 영속성 컨텍스트를 초기화한다(em.clear). 이후 조회한 내용들은 새롭게 수정된 DB에서 받아오게 된다.