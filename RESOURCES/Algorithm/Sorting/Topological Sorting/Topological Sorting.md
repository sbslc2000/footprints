# 위상 정렬
![](https://i.imgur.com/BRXpxSV.png)
위상정렬_Topological Sort_은 싸이클이 없는 유향 그래프 G=(V,E)에서 V의 모든 정점을 정렬하되 다음 성질을 만족해야한다.

- 간선 (i,j)가 존재하면 정렬 결과에서 정점 i 는 반드시 정점 j보다 앞에 나열되어야한다.

임의의 그래프에 대하여 위상정렬 결과는 여러개가 될 수 있다.
## 종류
[[Kahn's Algorithm]]
[[DFS 기반 위상정렬]]


## DFS 기반 