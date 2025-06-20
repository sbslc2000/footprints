---
상위 개념: "[[Serializability]]"
---
# Conflict Serializability
만약 스케줄 S가 한 직렬 스케줄과 [충돌 동등](Conflict%20Equivalent.md)하다면 그 스케줄 S는 충돌 직렬 가능성(Conflict Serializability)를 갖는다고 말한다.

![](https://i.imgur.com/QndMCbo.png)

위 스케줄은 충돌이 발생하지 않는 순서교환을 수행할 수 있다.
1. T1.read(B)와 T2.read(A)의 교환
2. T1.write(B)와 T2.write(A)의 교환
3. T1.write(B)와 T2.read(A)의 교환

교환을 모두 수행하면 다음과 같아진다.

![](https://i.imgur.com/MxNdshf.png)

최초 스케줄과 최종 스케줄은 충돌 동등이며, 최종 스케줄은 직렬 스케줄이므로, 최초 스케줄은 충돌 직렬 가능성을 갖는다.

## 검사하는 법
서로 다른 두 스케줄이 충돌 직렬 가능인지 검사할 때에는 **우선순위 그래프**(precedence graph)를 사용하면 효율적이다.

우선순위 그래프에서 정점은 스케줄에 참가하는 각 트랜잭션이며, 간선은 다음 조건에 따라 만들어진다.

```
간선 Ti -> Tj는 다음 조건이 만족될 때 존재한다.
1. Ti가 wirte(Q)를 실행한 다음 Tj가 read(Q)를 실행할 때
2. Ti가 read(Q)를 실행한 다음 Tj가 write(Q)를 실행할 때
3. Ti가 write(Q)를 실행한 다음 Tj가 write(Q)를 실행할 때
```

이렇게 만들어진 우선순위 그래프에 사이클이 존재한다면 해당 스케줄은 충돌 직렬 가능 스케줄이 아니다. 반대로 그래프에 사이클이 없다면 스케줄 S는 충돌 직렬 가능 스케줄이다.

만약 간선 $T_i \rightarrow T_J$가 우선순위 그래프에 존재한다면, 스케줄에서 $T_i$는 항상 $T_j$보다 먼저 실행되어야 한다. 더 나아가 스케줄 내의 트랜잭션의 직렬 가능성 순서(serrializability order)는 우선순위 그래프의 부분 순서에 대응되는 선형 순서를 결정하는 [위상 정렬](../../../Algorithm/Sorting/Topological%20Sorting/Topological%20Sorting.md)을 통해 얻을 수 있다.





### 예시 1. 충돌 직렬 가능성이 있는 예시
![](https://i.imgur.com/MxNdshf.png)
위 스케줄의 경우, 스케줄에 참가하는 트랜잭션은 $T_1$과 $T_2$이며, 간선 조건에 따라 $T_1$ -> $T_2$의 간선만 존재하게 되므로 그래프는 다음과 같다.

![](https://i.imgur.com/fDu70yo.png)

사이클이 없으므로 이는 충돌 직렬 가능 스케줄이다.

### 예시 2. 충돌 직렬 가능성이 없는 예시

![](https://i.imgur.com/51N8ETc.png)
위 스케줄에서 데이터 A에 대하여 $T_1$이 먼저 읽고, $T_2$가 나중에 쓰고 있으므로 간선 $T_1$ -> $T_2$가 존재한다. 또한 $T_2$가 먼저 읽은 B에 대하여 $T_1$이 쓰기를 수행하기에 간선 $T_2$ -> $T_1$역시 존재한다.
![](https://i.imgur.com/2NT31rm.png)
이 경우 그래프에 사이클이 존재하므로 해당 스케줄은 충돌 직렬 가능 스케줄이 아니다.