---
상위 개념: "[[Isolation Level]]"
---
# READ COMMITTED
READ COMMITTED 수준에서는 다른 트랜잭션에서 커밋한 데이터만 읽을 수 있다.

READ COMMITTED 수준은 Dirty Read 현상이 발생하지 않지만 [반복 불가능한 읽기](../Non-repeatbale%20Read.md) 문제와 [팬텀 현상](../Phantom%20Phenomenon.md) 문제가 발생할 수 있다.

READ COMMITTED 수준은 수준-2 일관성 규약으로 구현될 수 있다.