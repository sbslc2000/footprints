---
상위 개념: "[[Data Structure]]"
---
# Graph
![](https://i.imgur.com/RlvXtuJ.png)


그래프는 정점(Vertex)와 간선(Edge)로 이루어진 데이터 구조이다. 
  
## Graph의 표현

### 인접 행렬 Adjacency Matric
![](https://i.imgur.com/E2Ke0vj.png)
그래프를 N * N 의 행렬로 표현하는 방식이다.

2차원 행렬에서 원소 (i,j) 의 값은 그래프의 성격에 따라 여러 의미를 갖는다.
![](https://i.imgur.com/hfUJsPM.png)
![](https://i.imgur.com/41gtpUG.png)

무향 그래프에서 원소 (i,j) 가 1인 경우 정점 i, j 간에 간선이 존재함을 의미하며, 0 혹은 무한대인 경우 간선이 존재하지 않음을 의미한다. 무향 그래프의 특성 상 (i,j)가 1이라면 그 반대인 (j,i) 역시 1이어야 한다. 이 경우 N\*N 행렬은 우하향 대각선을 기준으로 대칭을 이룬다. (물론 표현은 표현 설계에 따라 달라질 수 있다.)

![](https://i.imgur.com/Y2keM9G.png)
유향 그래프에서 원소 (i,j)가 1이라면 정점 i 로부터 정점 j 로 연결되는 간선이 있음을 의미한다. 무향그래프와 다르게 이 경우 원소 (j,i) 는 0일 수 있다.

![](https://i.imgur.com/uERfN6u.png)
인접행렬에서 가중치 있는 그래프의 경우 원소의 값을 가중치로 활용할 수 있다.

인접행렬은 존재하지 않는 간선에 대해서도 0이라는 데이터를 유지해야 하기 때문에 정점에 비해서 간선의 수가 적다면 비효율적인 자료구조이다. 이 경우 **인접 리스트**를 활용하는 것이 효율적일 수 있다.

### 인접 리스트 Adjacency List
인접리스트란 그래프를 N개의 연결 리스트로 표현하는 방식이다. i 번째 리스트는 정점 i에 인접한 정점들을 리스트로 연결해 놓는다. 가중치의 경우 Node에 가중치를 저장할 수 있는 공간을 추가로 놓아 관리할 수 있다.
![](https://i.imgur.com/kxHN0IH.png)
![](https://i.imgur.com/rQ9mJdq.png)

### 인접 배열
인접배열이란 그래프를 N개의 연결배열로 표현한 것이다. i 번째 배열은 정점 i에 인접한 정점들의 집합이다.
![](https://i.imgur.com/xcKRdBS.png)
