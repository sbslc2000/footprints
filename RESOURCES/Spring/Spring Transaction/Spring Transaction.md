# Spring Transaction

## 제공하는 기능
![[Pasted image 20231115120631.png]]
* [[트랜잭션 추상화]]
* [[트랜잭션 동기화 매니저]]

## 스프링 트랜잭션 사용 방식
선언적 트랜잭션 관리*Declarative Transaction Management*란 @Transactional 애노테이션을 통해 해당 메서드의 동작이 하나의 트랜잭션으로 수행되어야 함을 선언하는 방식이다. 반면 프로그래밍 방식의 트랜잭션 관리*Programmatic Transcation Management*란 트랜잭션 매니저 혹은 트랜잭션 템플릿 등을 사용하여 트랜잭션 관련 코드를 직접 작성하는 방식을 의미한다.

프로그래밍 방식의 트랜잭션 관리를 사용하면 애플리케이션 코드가 트랜잭션 기술 코드와 강하게 결합된다.

## 트랜잭션과 AOP
@Transactional 애노테이션을 사용하게 되면 기본적으로 프록시 방식의 AOP가 적용된다.
![[Pasted image 20231115120402.png]]
트랜잭션 처리 로직을 갖고 있는 프록시 객체와 비즈니스 로직을 가진 서비스 객체를 분리해낼 수 있다. 이를 통해 서비스 계층에는 순수한 비즈니스 로직만을 남길 수 있다.

# @Transactional
[[@Transactional]]


## 트랜잭션 관련 로그 보는 방법
```
logging.level.org.springframework.transaction.interceptor=TRACE
```
애플리케이션 환경변수에 위 값을 넣어주면 transaction interceptor 가 trace 레벨로 호출하는 로그를 확인하여 트랜잭션의 시작과 끝을 확인할 수 있다.


## 트랜잭션 AOP 주의사항
[[트랜잭션 AOP 주의사항]]