---
상위 개념: "[[트랜잭션과 락]]"
---
# @Version
@Version을 사용하면 Entity에서 발생하는 동시성 문제를 해결할 수 있다.

## 동시성 문제 예시

Counter 엔티티는 count를 변수로 가지고 있으며, increase와 getter를 통해 count를 증가시키고 값을 조회할 수 있다.
```java
@Entity  
@Getter  
public class Counter {  
  
    @Id  
    @GeneratedValue    
    private Long id;  
  
    private int count;  
  
    public void increase() {  
       count++;  
    }  
}
```

다음과 같은 상황을 통해 서로다른 두 트랜잭션이 동시에 count를 증가시키는 상황을 가정해보자
```java
@BeforeEach  
void setUp() throws Exception {  
    EntityManager em = emf.createEntityManager();  
    EntityTransaction tx = em.getTransaction();  
    tx.begin();  
    Counter counter = new Counter();  
    em.persist(counter);  
    tx.commit();  
    em.close();  
}  
  
@Test  
void test() {  
    //start transaction  
    EntityManager em1 = emf.createEntityManager();  
    EntityManager em2 = emf.createEntityManager();  
  
    EntityTransaction tx1 = em1.getTransaction();  
    EntityTransaction tx2 = em2.getTransaction();  
  
    tx1.begin();  
    tx2.begin();  
  
    Counter counter1 = em1.find(Counter.class, 1L);  
    Counter counter2 = em2.find(Counter.class, 1L);  
  
    counter1.increase();  
    counter2.increase();  
  
    tx1.commit();  
    tx2.commit();  
  
    em1.close();  
    em2.close();  
  
    EntityManager em = emf.createEntityManager();  
    Counter counter = em.find(Counter.class, 1L);  
    log.info("count: {}", counter.getCount());  
}
```

위 코드를 실행시키면 아래와 같은 결과가 나온다. 
```
Hibernate: select c1_0.id,c1_0.count from counter c1_0 where c1_0.id=?
Hibernate: select c1_0.id,c1_0.count from counter c1_0 where c1_0.id=?
Hibernate: update counter set count=? where id=?
Hibernate: update counter set count=? where id=?
count: 1
```

#todo