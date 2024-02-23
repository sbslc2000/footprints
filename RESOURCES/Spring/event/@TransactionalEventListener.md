---
상위 개념: "[[@EventListener]]"
---
# @TransactionalEventListener
Spring 4.2 버전부터는 @EventListener를 확장한 @TransactionalEventListener를 통해 이벤트 리스너를 트랜잭션 단계에 바인딩 할 수 있게 해준다.

## TransactionPhase
@TransactionEventListener는 phase라는 속성을 통해 이벤트 발생 트랜잭션의 시기와 상태에 따라 이벤트 실행을 결정할 수 있다.

* **AFTER_COMMIT** : 기본값이며, 트랜잭션이 성공적으로 완료된 경우 이벤트를 발생시킨다.
* **AFTER_ROLLBACK** : 트랜잭션이 롤백된 경우 이벤트를 발생시킨다.
* **AFTER_COMPLEtION** : 트랜잭션이 종료된 경우 이벤트를 발생시키며, AFTER_COMMIT과 AFTER_ROLLBACK을 합친 속성이다.
* **BEFORE_COMMIT** : 트랜잭션이 종료되기 직전 이벤트를 발생시킨다.

@TransactionalEventListener은 오직 Event Producer 측에서 트랜잭션을 수행 중일 때에만 실행된다. 만약 트랜잭션이 수행중이지 않을 때에도 이벤트가 발생되기를 원한다면 fallbackExecution 속성을 true로 설정해주면 된다.

TransactionPhase는 동기-비동기와는 관련이 없다. 동기-비동기 설정을 하기 위해서는 별도의 @Async 애노테이션을 사용해줘야 하는 것으로 보인다.
