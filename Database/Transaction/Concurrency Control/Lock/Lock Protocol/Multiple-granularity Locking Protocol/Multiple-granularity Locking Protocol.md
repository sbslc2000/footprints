---
상위 개념: "[[../Lock Protocol|Lock Protocol]]"
---
# Multiple-granularity Locking Protocol
다중 세분도 잠금 규약(multiple-granularity locking protocol, MGL)은 데이터 항목에 대한 잠금을 시행하는 데 여러 단계의 세분도를 제공하는 잠금 규약 방식이다.

개별 데이터 항목을 잠금의 단위로 사용하면 대량의 레코드에 대한 잠금을 걸어야 하는 경우 너무 오랜 시간이 걸릴 수도 있고, 최악의 경우 잠금 테이블이 너무 커져 메모리의 크기를 초과할 수도 있다. 만약 잠금의 단위를 유연하게 만들어 하나의 잠금으로 데이터베이스나 릴레이션에 잠금을 걸 수 있다면 효과적일 것이다. 다중 세분도 잠금 규약은 이러한 이유로 잠금 단위에 여러 단계의 세분도를 제공한다.

## 동작 원리
데이터 항목들은 트리 형태로 표현된다.

![](https://i.imgur.com/ZAzuuu6.png)

여기서 $A_n$은 데이터베이스, $F_n$은 릴레이션, $r_{n_k}$는 레코드로 볼 수 있다. 만약 $F_b$에 공유 잠금을 걸면, $r_{b_k}$에는 암시적인 공유 잠금이 걸린다.

만약 $T_j$가 $F_b$에 대한 독점 잠금을 얻고 싶다면, 루트 노드로부터 $F_b$까지 노드를 조사하며 다른 트랜잭션으로부터 독점 혹은 공유 잠금이 걸렸는지 확인해야 한다. 모든 부모 노드가 잠금이 없을 때에 $T_j$는 $F_b$에 독점 잠금을 얻을 수 있다. 반면 공유 잠금을 얻고자 할 때에는, 부모 노드에 공유 잠금이 걸려있는 것은 허용할 것이다.

## Intention Lock Mode
만약 루트 노드에 공유 잠금을 얻고자 할 때는 어떻게 해야할까? 한 가지 방법은 전체 트리를 탐색하여 독점 잠금이 있는지 검사하는 것인데, 이는 다중 세분도 잠금 기법의 목적을 위배하는 것이다. 이를 효율적으로 수행하는 방법은 **의도 잠금 모드**(intention lock mode)라 불리는 새로운 형태의 잠금 모드를 도입하는 것이다.

의도 잠금 모드는 세 가지가 존재한다.
1. 의도-공유 모드(intention-shared mode) : 노드 N에 의도-공유 모드가 걸려있다면, 하위 노드에서 명시적으로 공유 모드로 잠금이 걸려있다는 것을 의미한다.
2. 의도-독점 모드(intention-exclusive mode) : 노드 N에 의도-독점 모드가 걸려있다면, 하위 노드에서 명시적으로 독점 또는 공유 모드로 잠금이 걸려있다는 것을 의미한다.
3. 공유와 의도-독점 모드(shared and intention-exclusive lock) : 노드 N에 공유와 의도-독점 모드 잠금이 걸려있다면, 그 노드를 루트로 하는 서브트리에 명시적으로 공유 모드 잠금이 걸리고, 그 노드를 포함하는 단계보다 낮은 단계에는 독점적 모드로 잠금이 걸려있다는 것을 의미한다.

만약 $T_i$가 $F_b$에 공유 잠금을 걸기 위하여 루트 부터 잠금을 조사할 때, 조사하는 노드마다 의도-공유 모드의 잠금을 부여하여 하위 노드에 공유 모드 잠금이 있음을 기록한다. 이러한 방식으로 의도-공유 잠금을 루트부터 $F_b$까지의 경로에 설정하면, 다른 트랜잭션이 동일한 경로 상의 상위 노드에 독점 잠금을 시도할 때, 해당 노드들에 이미 의도-공유 잠금이 걸려 있으므로 충돌을 감지할 수 있다. 이는 트랜잭션 간의 잠금 충돌 여부를 루트 경로 수준에서 빠르게 파악할 수 있게 해준다.

공유와 의도-독점 모드(shared and intention-exclusive mode)는 한 트랜잭션이 특정 노드에 대해 공유 잠금을 획득함과 동시에, 해당 노드를 루트로 하는 하위 트리의 일부 노드에 대해 독점 잠금을 획득하려는 상황에서 사용된다. 예를 들어, 트랜잭션 $T_k$가 릴레이션 $F_b$ 전체에 대해서는 공유 잠금을 부여받고, 그 하위에 위치한 특정 레코드 $r_{b_3}$에 대해서는 독점 잠금을 획득하려는 경우가 이에 해당한다.

이때 $T_k$는 $F_b$에 대해 공유와 의도-독점 모드를 설정하게 되며, 이는 두 가지 의미를 동시에 전달한다. 첫째, $F_b$ 자체에는 공유 잠금이 걸려 있음을 의미하며, 이는 다른 트랜잭션들이 해당 릴레이션을 읽을 수는 있지만 쓰기는 할 수 없음을 나타낸다. 둘째, $F_b$의 하위 노드들에는 독점 잠금이 설정될 예정임을 알리므로, 다른 트랜잭션이 상위 노드나 병렬 노드에 독점 또는 공유 잠금을 시도할 경우 적절히 충돌을 회피할 수 있다.

## 충돌 직렬성 보장을 위해서
다중 세분도 잠금 규약에서 반드시 하향식 순(루트에서 단말로)으로 잠금을 걸고 상향식 순(단말에서 루트로)으로 잠금을 해제해야 충돌 직렬성을 보장할 수 있다. 여기서 알 수 있다시피, 다중 세분도 잠금 규약은 [2-phase-locking](2-Phase%20Locking%20Protocol)의 일종이다. 이는 교착 상태 역시 발생할 수 있다는 것을 시사한다.

  
