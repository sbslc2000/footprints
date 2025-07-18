---
상위 개념: "[[../Concurrency Control|Concurrency Control]]"
---
# Degree-two consistency
수준-2 일관성(degree-two consistency)의 목적은 직렬 가능성을 반드시 보장하지는 않되, 연쇄적 롤백을 피하는 것이다. **트랜잭션은 데이터 항목에 접근할 때 적절한 잠금 모드를 가지고 있어야 하지만, 2단계 방식을 요구하지는 않는다.** 

공유 잠금은 언제든 해제할 수 있고 걸 수 있다. 그러나 독점적 잠금은 트랜잭션이 커밋하거나 롤백하기 전에는 해제할 수 없다. 이로 인해 직렬 가능성은 일관성 수준에서 보장되지 않으며, 트랜잭션이 같은 데이터 항목에 두 번 접근해도 각각 다른 결과를 얻을 수 있다. 이는 [READ COMMITTED](../../Isolation%20Level/READ%20COMMITTED.md) 수준을 구현한 것이라 할 수 있다. (커밋된 데이터만 읽지만, 반복 불가능 읽기와 팬텀 현상은 발생)