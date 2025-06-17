---
상위 개념: "[[Isolation Level]]"
---
# READ UNCOMMITTED
READ UNCOMMITTED 수준에서는 다른 트랜잭션에서 변경하고 아직 커밋하지 않은 데이터를 조회할 수 있다. 매우 높은 수준의 동시성을 누리는 반면 일관성을 포기해야한다.

READ UNCOMMITTED에서는 다음과 같은 문제가 발생할 수 있다.
* [[Dirty Read]]
* [[Non-repeatbale Read]]
* [[Phantom Phenomenon]]