영속성 컨텍스트란 "엔티티를 영구 저장하는 환경"이라는 뜻으로, JPA를 이해하기 위해 가장 중요한 용어입니다.

# EntityManager
JPA를 사용하면 Entity Manager를 통해 Persistence 기능을 사용할 수 있다. EntityManager는 Entity 영속화와 관련된 다양한 기능을 수행한다.


# 엔티티의 생명주기
[[엔티티의 생명주기]]


# 영속성 컨텍스트의 이점
* 1차 캐시
* 동일성 보장
* 트랜잭션을 지원하는 쓰기 지연
* 변경 감지*Dirty Checking*
* 지연 로딩*Lazy Loading*

## 1차 캐시
EntityManager를 사용하면 영속성 컨텍스트에 Id와 Entity 정보를 갖고 있는 캐시영역이 생긴다. em.persist는 먼저 1차 캐시에 저장되며, 이후 em.find로 해당 객체를 가져올 때는 DB가 아닌 캐시로부터 가져온다.
```java
Member member = new Member(1L,"member1");
//1차 캐시에 저장
em.persist(member);

//1차 캐시로부터 가져옴
//따라서, 조회용 쿼리가 나가지 않음
Member findMember = em.find(Member.class, member.getId());
```

```java
//DB에서 객체를 가져오며, 이를 1차 캐시에 저장한다.
Member findMember = em.find(Member.class, 1L);

//1L은 이미 캐시에 있으므로 조회 쿼리 사용하지 않고 가져옴
Member findMember2 = em.find(Member.class, 1L);
```

1차 캐시는 하나의 트랜잭션에서만 적용되기 때문에 성능향상에 있어서 큰 이점을 갖지는 않는다.
## 영속 엔티티의 동일성 보장
동일한 영속 엔티티들은 == 연산자로 비교할 시 true를 반환, 단 같은 트랜잭션 안에 있어야 함.
```java
Member findMember1 = em.find(Member.class,1L);
Member findMember2 = em.find(Member.class,1L);

if(findMember1 == findMember2) // == 비교 가능 
	System.out.println("True")
```
## 트랜잭션을 지원하는 쓰기 지연
em.persist()를 사용하면 insert query는 바로 DB에 전달되지 않고 쓰기 지연 저장소에 쌓이며, 이후 transaction.commit()이 수행되면 DB에 전달된다.
=> **버퍼링**을 통한 성능 향상 가능. transaction 중단 시 rollback 쿼리 보내지 않아도 됨.
```java
em.persist(memberA);
em.persist(memberB);

tx.commit();
```
![[Pasted image 20230922115521.png]]
![[Pasted image 20230922115534.png]]

## 변경 감지*Dirty Checking*
[[Dirty Checking]]
변경 감지 방식이 아닌 병합 방식으로도 엔티티를 수정할 수 있다.
# 플러시
영속성 컨텍스트의 변경내용을 데이터베이스에 반영하는 것.  
플러시가 발생하면 다음과 같은 일들을 진행한다.
* Dirty Checking
* 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
* 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송 (insert, update, delete...)
## 영속성 컨텍스트를 플러시하는 방법
* em.flush() 직접 호출
* 트랜잭션 커밋 -- em.flush 자동 호출
* JPQL 쿼리 실행 -- em.flush 자동 호출
	* JPQL은 동기화문제로 먼저 flush가 진행된 뒤 수행된다.
## 플러시 모드 옵션
`em.setFlushMode(FlushModeType.COMMIT);`
* `FlushModeType.AUTO` : 커밋이나 쿼리를 실행할 때 플러시 (기본값)
* `FlushModeType.COMMIT` : 커밋할 때만 플러시


