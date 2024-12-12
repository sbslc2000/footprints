ToMany 관계에서 join fetch를 사용하면 데이터 뻥튀기 현상이 발생하므로 페이징이 곤란해진다. JPA에서 이런 문제를 해결하기 위한 방안이 있다.
1. ToOne 관계를 모두 fetch join으로 가져온다.
2. 컬렉션은 지연로딩으로 조회한다.
3. 지연 로딩 성능 최적화를 위해 hibernate.default_batch_fetch_size, @BatchSize를 적용한다.

	1. hibernate.default_batch_fetch_size

```yml
spring:
	jpa:
		properties:
			hibernate:
				default_batch_fetch_size: 100
```

이 경우 루프를 통해 lazy loading이 발생하는 경우 batch size만큼의 데이터를 한번에 가져온다. 내부적으로는 IN 키워드를 사용함.

```java
public class Order {
...
	@BatchSize(size=1000)  
	@OneToMany(mappedBy = "order", cascade = CascadeType.ALL)  
	private List<OrderItem> orderItems = new ArrayList<>();
	}

```

위와 같이 BatchSize를 연관관계 필드 레벨에 작성할 수도 있다.