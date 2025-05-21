---
상위 개념: "[[2-Phase Locking Protocol]]"
---
# Rigorous 2-phase Locking
준엄한 2단계 잠금 규약(Rigorous 2-phase Locking)은 트랜잭션이 커밋할 때 까지 공유 및 독점적 잠금을 해제하지 않고 계속 유지되게 하는 잠금 규약이다.

대부분의 상업용 데이터베이스는 잠금 변환이 있는 준엄한 2단계 잠금 규약을 구현하나, 단순히 2단계 잠금 규약이라고만 표현한다.