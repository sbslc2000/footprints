---
상위 개념: "[[Distance Vector Algorithm]]"
---
# Poisoned Reverse
포이즌 리버스(poisoned reverse)는 라우팅 루프 시나리오를 방지하는 기법이다.

만약 라우터 x가 y를 통해 z로 가는 경로 설정을 했다면, x는 y에게 z까지의 거리가 무한대라고 알린다. 이는 y가 z로 가기 위해 x를 선택하는 일을 방지한다.

포이즌 리버스는 3개 이상의 노드를 포함한 루프를 감지할 수 없다.

