# 프록시 내부 호출 문제

![](https://i.imgur.com/hsfnhZ1.png)

만약 서비스 코드에서 external 메서드가 internal 메서드를 호출하는 경우 internal에 대한 호출은 프록시를 거치지 않으므로 @Transactional이 붙어있더라도 트랜잭션이 적용되지 않는 문제가 발생할 수 있다.
```java
@Test  
void internalCall() {  
    callService.internal();  //트랜잭션 적용 o
}  
  
@Test  
void externalCall() {  
    callService.external();   //트랜잭션 적용 x
}  
  
static class CallService {  
  
    public void external() {  
        log.info("call external");  
        printTxInfo();  
        internal();  //이 코드는 프록시를 거치지 않음
    }  
  
    @Transactional  
    public void internal() {  
        log.info("call internal");  
        printTxInfo();  
    }  

}
```

## 해결방법
### 별도 클래스 분리
```java
@RequiredArgsConstructor  
static class CallService {  
      
    private final InternalService internalService;  
  
    public void external() {  
        log.info("call external");  
        printTxInfo();  
        internalService.internal();  //트랜잭션 적용 o
    }  
}  
  
static class InternalService {  
      
    @Transactional  
    public void internal() {  
        log.info("call internal");  
        printTxInfo();  
    }  
}
```


# Public 메서드에만 트랜잭션 적용
스프링 트랜잭션은 public 메서드에만 트랜잭션을 적용하도록 기본 설정이 되어있다. 접근 제어자 중 protected, package-visible은 외부에서 호출 가능하므로 이 경우 트랜잭션이 적용되지 않는 것을 인지하고 있어야 한다.

모든 메서드에 트랜잭션을 적용하면 의도하지 않은 결과를 낳을 수 있다. Lambda 함수에 트랜잭션이 걸릴 수도 있고, private 메서드에 트랜잭션이 걸릴 수도 있다. 일반적으로 트랜잭션이 시작되는 지점은 서비스 로직이 시작되는 시점이므로 public 메서드에만 적용시킨다고 하더라도 큰 문제가 발생하지 않는다.

# 초기화 시점
스프링 초기화 시점에는 트랜잭션이 적용되지 않는다.