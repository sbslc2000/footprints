---
상위 개념: "[[Concurrency Control]]"
---
# Isolation Level
고립 수준(Isolation Level)이란 트랜잭션끼리 얼마나 서로 고립되어있는가를 나타내는 수준이다. 격리 수준이 높다면 트랜잭션은 거의 차례대로 수행되며 동시성 처리 성능이 떨어지지만 이상현상이 발생하지 않고, 낮은 격리 수준일수록 동시 처리 성능은 증가하지만 이상현상이 발생할 가능성이 생긴다.

동시성 수준이 높은 순서대로 고립 수준을 나열하면 READ UNCOMMITTED, READ COMMITTED, REPETABLE READ, SERIALIZABLE이다.

## In SQL
많은 데이터베이스 시스템은 기본적으로 READ COMMITTED 수준으로 동작한다. 시스템 기본 값을 사용하지 않고 명시적으로 고립성 수준을 설정할 수도 있다.
```sql
set transaction isolation level serializable
```
