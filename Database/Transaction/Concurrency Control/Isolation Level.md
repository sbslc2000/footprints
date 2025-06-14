---
상위 개념: "[[Concurrency Control]]"
---
# Isolation Level
Isolation Level (고립 수준, 격리 수준)이란 트랜잭션끼리 얼마나 서로 고립되어있는가를 나타내는 수준이다. 격리 수준이 높다면 트랜잭션은 거의 차례대로 수행되며 동시성 처리 성능이 떨어지지만 이상현상이 발생하지 않고, 낮은 격리 수준일수록 동시 처리 성능은 증가하지만 이상현상이 발생할 가능성이 생긴다.

## 종류
격리 수준이 낮은 순서대로 기록하였다.

### READ UNCOMMITTED
READ UNCOMMITTED 수준에선 다른 트랜잭션에서 변경하고 아직 커밋하지 않은 데이터를 조회할 수 있다. 이렇게 아직 커밋되지 않은 변경사항을 조회하는 것을 **Dirty Read**라고 하며, Dirty Read 데이터를 사용하여 작업할 때에 데이터를 변경한 측의 트랜잭션이 롤백 된다면 데이터 정합성에 문제가 발생할 수 있다.

### READ COMMITTED
READ COMMITTED 수준에서는 다른 트랜잭션에서 커밋한 데이터만 읽을 수 있다. 하지만 이 수준에서는 **NON-REPEATABLE READ** 문제가 발생한다. 

NON-REPEATABLE READ란 한 트랜잭션에서 A를 조회한 상태에서 다른 트랜잭션이 A를 변경하고 커밋까지 한 경우, 다시 A를 조회할 때에는 수정된 데이터를 읽게 되는 문제를 말한다. 이 경우 다시 조회했을 때 이전과 다른 데이터를 읽게 된다.

READ COMMITTED 수준은 수준-2 일관성 규약으로 구현될 수 있다.

### REPEATABLE READ
REAPEATABLE READ 수준에서는 한 번 조회한 데이터는 반복해서 조회해도 같은 데이터가 조회된다. 하지만 **PHANTOM READ** 문제는 발생할 수 있다. 

PHANTOM READ란 반복 조회시 결과 집합이 달라지는 것을 의미하며, 한 트랜잭션이 어떠한 조건에 따른 집합을 조회할 때 그 사이에 다른 트랜잭션이 집합에 요소를 추가한 경우에 해당한다.

### SERIALIZABLE
SERIALIZABLE은 가장 엄격한 트랜잭션 격리 수준이며 PHANTOM READ가 발생하지 않는다. 하지만 동시성 처리 성능이 급격히 떨어질 수 있다.

## In SQL
많은 데이터베이스 시스템은 기본적으로 READ COMMITTED 수준으로 동작한다. 시스템 기본 값을 사용하지 않고 명시적으로 고립성 수준을 설정할 수도 있다.
```sql
set transaction isolation level serializable
```
