```java
public class Member {
	private List<Order>orders = new ArrayList();
}
```

Hibernate는 엔티티를 영속화 할 때, 컬렉션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경한다.
```java
Member member = new Member();
System.out.println(member.getOrders().getClass());
//java.util.ArrayList

em.persist(member);
System.out.println(member.getOrders().getClass());
// org.hibernate.collection.internal.PersistentBag
```
만약 임의의 메서드에서 컬렉션을 잘못 설정하면 하이버네이트 내부 매커니즘에 문제가 발생할 수 있으므로, 필드레벨에서 생성하는 것이 가장 안전하고, 코드도 간결하다. 절대 매핑되는 컬렉션을 함부로 조작하지 말자!