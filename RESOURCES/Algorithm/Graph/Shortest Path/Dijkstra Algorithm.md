# Dijkstra Algorithm
다익스트라 알고리즘은 간선들의 가중치가 모두 0 이상인 경우에 해결할 수 있는 알고리즘이다.

## Pseudo Code
![](https://i.imgur.com/jx0Mq2v.png)

## 예시
![](https://i.imgur.com/NuX2s52.png)
(b)에서 집합 S에 0이 추가되고, 0과 인접한 정점들의 거리를 이완Relaxation 한다. 이후 정점들 중 비용이 가장 적은 정점을 집합 S에 포함시키고, 이완하는 작업을 반복한다.
![](https://i.imgur.com/XCOcTOf.png)
만약 인접 정점 v가 집합 S에 포함되어있지 않고, 시작정점으로부터 u까지 가는데 필요한 거리에서 u에서 v로 이동하는데 드는 비용을 합한 것이 기존에 유지했던 v까지 가는 거리 비용보다 작다면, 전자의 비용을 정점에 기록한다. [프림 알고리즘](Prim%20Algorithm)과 유사하다. 시간복잡도도 프림 알고리즘과 동일한 O(E log V)이다.