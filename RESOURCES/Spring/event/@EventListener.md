---
상위 개념: "[[Spring]]"
---

# @EventListener
[이벤트 기반 프로그래밍](Event-driven%20Programming.md)*Event-driven Programming*을 도와주는 스프링의 애노테이션이다.

다음 내용들은 스프링 4.2 버전부터 가능한 기술들이다.
## 구성요소
EventListener를 사용하기 위해서는 세 구성요소가 필요하다.
1. 이벤트 객체
2. 이벤트 발행자
3. 이벤트 구독자

### 이벤트 객체
이벤트 객체는 순수한 자바 객체로 만들어진다.
```java
@Getter  
public class MyEvent {  

    private final String message;  
    
    public MyEvent(String message) {  
        this.message = message;  
    }  
}
```

### 이벤트 발행자
이벤트를 발행하고자 하는 측에서는 ApplicationEventPublisher의 구현체를 주입받아 사용할 수 있다.
```java
@Slf4j  
@Service  
@RequiredArgsConstructor  
public class EventPublishingService {  
  
    private final ApplicationEventPublisher publisher;  
  
    public void publishEvent(String message) {  
        MyEvent event = new MyEvent(message);  
        log.info("Publishing Event, message = {}", message);  
  
        publisher.publishEvent(event);  
    }  
}
```

### 이벤트 구독자
이벤트 구독자 측에서는 @EventListener 애노테이션을 메서드에 작성하고 파라미터로 Event 객체를 받아서 해당 이벤트 발생시 코드를 수행시킬 수 있다.
```java
@Slf4j  
@Service  
public class EventHandlingService {  
    @EventListener  
    public void handle(MyEvent event) {  
        log.info("Received Event, message = {}", event.getMessage());  
    }  
}
```

### 테스트
```java
@Test  
void publishEvent() {  
    eventPublishingService.publishEvent("Hello Event!");  
}

//2023-11-16 14:37:26.500  INFO 48224 --- [    Test worker] c.e.demo.event.EventPublishingService    : Publishing Event, message = Hello World!
//2023-11-16 14:37:26.501  INFO 48224 --- [    Test worker] c.e.demo.event.EventHandlingService      : Received Event, message = Hello World!
```

## 언제 사용하면 좋을까?
한 서비스 메서드에서 부가적으로 수행되어야 하는 일들이 다른 여러 서비스 레벨에 전파된다면, 시작점의 서비스 메서드는 다른 일들을 수행하기 위해 여러 서비스 컴포넌트를 의존하고 직접 메서드를 호출해야할 것이다. 이는 부가적으로 수행되어야 하는 태스크들에 대한 직접적인 의존성을 만들어 낸다.

한편 이러한 문제를 이벤트 기반 프로그래밍으로 풀어낼 경우, 시작점 서비스 메서드와 부가적인 메서드들은 이벤트라는 중간자를 통해 호출되므로 의존이 약해지는 결과를 낳을 수 있다.  부가적인 메서드가 추가되거나 삭제되어야 할 때 시작점 서비스 메서드를 수정해야 하는 일이 발생하지 않을 것이다.

## 참조
위 기능 말고도 이벤트 리스너의 순서 지정과 비동기 수행 등 다양한 옵션을 제공한다.
https://brunch.co.kr/@springboot/422