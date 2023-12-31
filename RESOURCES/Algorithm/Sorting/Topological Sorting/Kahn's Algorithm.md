---
상위 개념: "[[Topological Sorting]]"
---
## Kahn's Algorithm
Kahn’s Algorithm은 진입간선과 진출간선에 대해 계산한 결과를 바탕으로 정렬하는 기술이다.
### Pseudo Code
![](https://i.imgur.com/Hch5ESV.png)
진입간선과 진출간선에 대해 계산한 결과를 바탕으로 진입간선이 없는 정점을 선택하여 배열에 배치한다.

배열에 배치할 때에는 해당 정점의 진출간선이자 다른 정점의 입장에서는 진입간선인 간선을 제거한다. 이를 통해 진입간선이 없는 새로운 정점이 생기게 되고 이는 배열에 배치될 대상이 된다.

for 루프는 정점의 개수만큼 반복되고, 각 간선의 제거과정은 간선의 개수만큼 반복될 것이기 때문에 총 수행시간은 $O(V+E)$이다.

### 예시

![](https://i.imgur.com/HCQK0ZI.png)
![](https://i.imgur.com/dM49bYg.png)

#위상정렬