---
상위 개념: "[[Local Search]]"
---
# Local Optima
![](https://i.imgur.com/19XBqiT.png)

지역 최적해(Local Optima)란 탐색 공간 안에서 일시적으로 가장 좋은 해처럼 보이지만, 전체적으로는 더 나은 해가 존재하는 상태를 의미한다.

HC나 Gradient Descent와 같은 탐색 알고리즘은 이웃보다 좋은 해가 없다면 멈추는 방식이기 때문에 Local Optimum에 도달하면 탐색이 조기 종료되고, 최적해를 산출하지 못하는 문제가 생긴다.

이를 해결하기 위해서는 나쁜 해도 수용하거나, 여러 시작점에서 탐색을 시작할 수 있다.

