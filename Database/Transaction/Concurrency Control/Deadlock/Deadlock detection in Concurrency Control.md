---
상위 개념: "[[Deadlock in Concurrency Control]]"
---
# Deadlock detection in Concurrency Control
교착 상태는 대기 그래프(wait-for graph)라고 불리는 방향성 그래프를 이용하여 표현할 수 있다. $G = (V, E)$에서 각 정점은 트랜잭션이고, 간선 E는 $T_i \rightarrow T_j$로 표현되는데, 이는 $T_i$가 필요한 데이터 항목을 $T_j$가 이미 점유하고 있어 놓을 때까지 기다려야 한다는 것을 의미한다.
![](https://i.imgur.com/cvCcKWC.png)

이 그래프에 순환이 존재하면 시스템에 교착 상태가 발생했음을 의미하여 그 사이클에 포함된 각 트랜잭션이 교착 상태에 빠졌다고 볼 수 있다. 데이터베이스 시스템은 교착 상태를 감지하기 위해 대기 그래프를 관리해야하며 주기적으로 그래프에 사이클이 발생했는지 검사하는 알고리즘을 수행해야 한다.