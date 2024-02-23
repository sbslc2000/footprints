특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함께 영속 상태로 만들고 싶을 때 사용

```java
@Entity
class Parent {
	...
	@OneToMany(mappedBy="parent", cascade = CascadeType.ALL)
	private List<Child> children = new ArrayList<>();	
}

@Entity
class Child {
	...
	@ManyToOne()
	@JoinColumn("PARENT_ID")
	private Parent parent;
}


...
	public static void main() {
		...
		Parent parent = new Parent();
		Child child1 = new Child();
		Child child2 = new Child();
		
		parent.addChild(child1);
		parent.addChild(child2);
		
		//영속성 전이가 되어있지 않다면 각각의 엔티티를 다 저장해야 함
		//em.persist(parent);
		//em.persist(child1);
		//em.persist(chidl2);
		
		//영속성 전이가 되어있다면 부모만 저장해도 모두 영속화됨
		em.persist(parent);
	}
```

## 주의사항
* 영속성 전이는 연관관계 매핑하는 것과는 아무 관련이 없음
* 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 편의 기능만 제공
* Parent와 Child 둘 간에만 연관관계가 있을 때 사용하기 유리. (소유자가 하나일 때)

## CascadeType의 종류
* **ALL** : 모두 적용
* **PERSIST** : 영속화 할때만 적용
* **REMOVE** : 삭제시만 적용
* MERGE, REFRESH, DETACH