# @Transactional
@Transactional은 스프링에서 선언적 트랜잭션 적용을 위해 사용하는 애노테이션이다.

```java
 @Transactional        
 public void tx() {
     log.info("call tx");
     boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
     log.info("tx active={}", txActive);
 }
```

## 트랜잭션 적용 위치
1. 우선순위
트랜잭션을 사용할 때에는 다양한 옵션을 사용할 수 있는데, 항상 더 구체적인 부분에 적용된 애노테이션이 더 높은 우선순위를 갖는다.

메서드와 클래스에 애노테이션을 붙인다면 메서드가 더 높은 우선순위를 갖는다. 인터페이스와 구현 클래스에 애노테이션을 붙일 수 있다면 클래스가 더 높은 우선순위를 갖는다.

2. 클래스에 적용하면 메서드는 자동 적용

```java
@Test  
void orderTest() {  
    levelService.write();  
    levelService.read();  
}  
  
@Slf4j  
@Transactional(readOnly = true)  
static class LevelService {  
    //readOnly = false 의 트랜잭션이 적용  
    @Transactional(readOnly = false)  
    public void write() {  
        log.info("call write");  
        printTxInfo(); // true, false  
    }  
  
    //readOnly = true 의 트랜잭션이 적용  
    public void read() {  
        log.info("call read");  
        printTxInfo(); // true, true  
    }  
  
    private void printTxInfo() {  
        boolean active = TransactionSynchronizationManager.isActualTransactionActive();  
        boolean readOnly = TransactionSynchronizationManager.isCurrentTransactionReadOnly();  
        log.info("active = {}", active);  
        log.info("readOnly = {}", readOnly);  
    }  
}
```

 ### 인터페이스에 @Transactional 이 사용되는 경우
 인터페이스에 @Transactional 이 사용되는 경우 다음과 같은 우선순위를 갖는다.
 1. 클래스의 메서드
 2. 클래스의 타입
 3. 인터페이스의 메서드
 4. 인터페이스의 타입

인터페이스에 @Transactional을 사용하는 것은 스프링 공식 매뉴얼에서 권장하는 방법은 아니다. 가급적 구체 클래스에 @Transactional을 사용하는 것이 좋다.

# 옵션
[[@Transactional 옵션]]
