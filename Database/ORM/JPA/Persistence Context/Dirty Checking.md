JPA에서 Dirty Checking이란, 영속성 컨텍스트로 관리되는 엔티티들의 초기 상태를 기록해두고 있다가 트랜잭션이 commit되는 시점에 초기 상태와 변경된 상태를 비교하여 쿼리를 만들고 DB에 변경사항을 반영하는 방법이다. 

Dirty Checking을 통해 하나의 작업 단위에서 발생하는 변경 쿼리들을 모아놓고 한 번의 커넥션을 통해 전달하므로 성능상의 이점이 발생하며, 트랜잭션 롤백 시 애플리케이션 레벨에서 모아놨던 쿼리를 폐기하는 방식으로 동작하므로 DB를 효율적으로 사용할 수 있다.

```java
Member findMember = em.find(Member.class, 150L);
member.setName("newName");
//em.persist(member); 이런 코드 필요 없음
//커밋 시점에서 자동으로 변경사항 반영

transaction.commit();
```

![[Pasted image 20230922121024.png]]