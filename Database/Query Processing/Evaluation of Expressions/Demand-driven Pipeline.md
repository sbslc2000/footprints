---
상위 개념: "[[Pipelining Evaluation]]"
---
# Demand-driven Pipeline
요구 구동 파이프라인은 상층부에 있는 연산이 하층부에게 입력 튜플을 달라고 요청하는 것을 반복한다. 요청이 들어올 때, 하층부는 튜플이 필요하다면 그의 하층부에게 튜플을 요청하고, 연산을 한 뒤 그 결과를 상층부에 넘겨준다.

하층부에 대한 출력 요청 (하층부의 입장에서는 출력 요청, 상층부의 입장에서는 입력 요청)은 상층부가 필요할 때 요청하고, 하층부는 요구가 있을 때에만(lazily) 튜플을 만든다. 구현이 비교적 간단하기에 많이 사용된다.