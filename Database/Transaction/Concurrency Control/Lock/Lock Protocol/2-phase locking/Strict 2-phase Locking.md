---
상위 개념: "[[Lock Protocol/2-phase locking/2-Phase Locking Protocol]]"
---
# Strict 2-phase Locking
엄격한 2단계 잠금 규약(Strict 2-phase Locking)은 잠금을 2단계로 수행할 뿐만 아니라, 트랜잭션이 정상적으로 커밋할 때까지 자신이 가진 독점적 잠금을 계속 유지하도록 한다.

이에 따라 아직 커밋하지 않은 트랜잭션에 의해 갱신된 데이터는 그 트랜잭션이 커밋할 때까지 독점적 모드로 잠금이 걸려있게 되며, 다른 트랜잭션이 그 데이터를 읽을 수 없다는 것을 보장한다.[이는 연쇄적인 롤백을 방지하는 효과를 갖는다.](../../../../Schedule/Cascadeless%20Schedule.md) 