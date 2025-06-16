---
상위 개념: "[[JDBC]]"
---
# Conntection
JDBC의 Connection 클래스는 데이터베이스와 연결된 세션을 나타내는 객체를 표현한다.

JDBC를 통해 SQL을 실행하기 위해서는 반드시 먼저 Connection을 확보해야 하며, 이 객체를 통해 SQL 명령 실행, 트랜잭션 처리, 자원 해제가 이루어진다.

## API

### setTransactionIsolation(int level)
`setTransactionIsolation(int level)` 메서드는 커넥션의 [트랜잭션 고립성 수준](../../../Database/Transaction/Concurrency%20Control/Isolation%20Level/Isolation%20Level.md)을 설정한다.

* Connection.TRANSACTION_SERIALIZABLE
* Connection.TRANSACTION_REPEATABLE_READ
* Connection.TRANSACTION_READ_COMMITTED
* Connection.TRANSACTION_READ_UNCOMMITTED