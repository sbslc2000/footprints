# Conntection

## API

### setTransactionIsolation(int level)
`setTransactionIsolation(int level)` 메서드는 커넥션의 [트랜잭션 고립성 수준](../../../Database/Transaction/Concurrency%20Control/Isolation%20Level.md)을 설정한다.

* Connection.TRANSACTION_SERIALIZABLE
* Connection.TRANSACTION_REPEATABLE_READ
* Connection.TRANSACTION_READ_COMMITTED
* Connection.TRANSACTION_READ_UNCOMMITTED