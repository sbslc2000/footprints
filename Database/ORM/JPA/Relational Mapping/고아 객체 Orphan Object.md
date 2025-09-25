---
상위 링크: "[[../Persistence Context/Persistence Context]]"
---
고아 객체 제거 : 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제하는 기능

```java
@Entity
class Parent {
	...
	@OneToMany(mappedBy="parent", orphanRemoval = true)
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
		
		em.flush();
		em.clear(); //영속성 컨텍스트 초기화
		
		Parent findParent = em.find(Parent.class, parent.getId());
		findParent.getChildren().remove(child1); //DELETE 쿼리 발생
	}
```

## 주의점
* 참조하는 곳이 하나일 때에만 사용해야함
* 특정 엔티티가 개인 소유할 때 사용
* @OneToOne, @OneToMany만 사용
* 부모 객체가 삭제되면, 자식 객체도 함께 삭제된다.

