---
상위 개념: "[[../Lock Protocol|Lock Protocol]]"
---
# Graph-based Protocol
그래프 기반 규약(Graph-based Protocol)은 충돌 직렬 가능성을 보장하는 잠금 규약 중 하나이다.

그래프 기반 규약은 모든 데이터 항목의 집합에 부분적인 순서를 부여 한다. 예를 들어 집합 $D = \{d_1, d_2, \ldots , d_h \}$ 에 부분 순서 $d_i \rightarrow d_j$ 가 있을 수 있다. 이 때 $d_i$와 $d_j$에 모두 접근해야하는 트랜잭션은 $d_j$에 접근하기 전에 반드시 $d_i$에 접근해야 한다.

![](https://i.imgur.com/jZyE6W4.png)

부분적 순서는 집합 D가 데이터베이스 그래프(database graph)라고 불리는 비순환 그래프로 나타낼 수 있다는 것을 의미한다. 해당 그래프에 대해 그래프 기반 규약은 트랜잭션이 다음 규칙을 따르도록 강제한다. 다음 규약을 따르는 정당한 스케줄은 모두 충돌 직렬 가능성을 갖는다.

1. 트랜잭션 $T_i$의 첫 번째 잠금은 어떠한 데이터 항목에도 걸 수 있다.
2. 트랜잭션 $T_i$는 Q의 부모에 해당하는 데이터 항목에 이미 잠금을 걸고 있는 경우에만 Q에 잠금을 걸 수 있다.
3. 데이터 항목의 잠금은 언제든지 해제할 수 있다.
4. $T_i$가 잠금을 걸었다가 해제한 데이터 항목은 다시 $T_i$가 잠금을 걸 수 없다.

트리 잠금 규약은 교착 상태가 발생하지 않아 롤백이 필요없다는 장점이 있으나, 때에 따라 트랜잭션 자신이 접근하지 않는 데이터 항목에도 잠금을 걸게 한다는 단점이 있다.



