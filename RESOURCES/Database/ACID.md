
# ACID

**Atomicity** : 트랜잭션 내에서 실행한 작업들은 마치 하나의 작업인 것처럼 모두 성공하거나 모두 실패해야한다.
**Consistency** : 모든 트랜잭션은 일관성있는 데이터베이스 상태를 유지해야 한다. 
**Isolation** : 동시에 실행되는 트랜잭션들은 서로에게 영향을 미치지 않도록 격리해야한다.
**Durability** : 트랜잭션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템에 문제가 발생하더라도 로그를 사용하여 성공한 트랜잭션 내용을 복구할 수 있어야 한다. 

이 중 Isolation을 트랜잭션간에 완벽히 보장하려면 트랜잭션을 거의 순서대로 실행해야 한다. 이렇게 하면 동시처리 성능이 매우 나빠진다. 이런 문제로 인해 ANSI 표준에서는 트랜잭션의 격리 수준을 4단계로 나누어 정리했다.

## Isolation Level
#todo <!-- 채우기 -->
* READ UNCOMMITED 
* READ COMMITED
* REPEATABLE READ
* SERIALIZABLE


[[Transaction]]