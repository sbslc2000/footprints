---
상위 개념: "[[JpaRepository]]"
---
## JpaRepository delete에서 merge가 수행되는 이유
SimpleJpaRepository의 delete 메서드 내부 코드는 다음과 같이 작성되어있습니다.
```java
@Override  
@Transactional  
public void delete(T entity) {  
  ...
  
    entityManager.remove(entityManager.contains(entity) ? entity : entityManager.merge(entity));  
}
```

JPARepository를 통해 엔티티 삭제를 수행할 때 삭제 대상이 영속성 컨텍스트의 관리중이 아니라면 DB로부터 값을 가져와 영속성 컨텍스트에 포함시킨 뒤 remove 메서드를 호출합니다. 이러한 코드에 의하여 만약 준영속 상태의 엔티티를 삭제한다면 select 쿼리와 delete 쿼리가 순차적으로 수행됩니다.

이러한 다소 불필요해보이는 작업을 하는 이유는 영속성 컨텍스트가 제공하는 기능을 누리기 위함입니다.

만약 작업 도중 예외가 발생하여 트랜잭션이 롤백되어야하는 경우 삭제 쿼리 역시 함께 롤백되어야 할 것이다. 이러한 작업은 일반적인 경우 영속성 컨텍스트가 flush 되지 않음으로써 쿼리가 수행되지 않아 트랜잭션의 효과를 얻게 되는데, merge를 하지 않고 삭제 쿼리를 바로 날리는 경우 이러한 효과를 누리기 어려울 수 있다.

또한 cascade가 설정되어있는 경우 영속성 컨텍스트에 포함이 되어있어야만 연쇄된 삭제 효과를 가질 수 있다. 이러한 이유로 JpaRepository는 단순히 ID를 통해 삭제 연산을 수행하더라도 DB로부터 값을 가져와 먼저 엔티티를 영속화하는 과정을 거치게 된다.