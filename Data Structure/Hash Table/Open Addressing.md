---
상위 개념: "[[Hash Table]]"
---
# Open Addressing
개방 주소법(Open Addressing)이란 해시 충돌이 일어나도 어떻게든 주어진 테이블 공간에서 해결하는 방법이다. 추가적인 공간을 요구하지 않으며, 빈자리가 생길 때 까지 해시값을 연속해서 만들어낸다.

1. Linear Probing

![](https://i.imgur.com/tibVxNa.png)
$h_i(x) = (h(x)+i)\ \ mod\ \ m$ 과 같은 방법으로 N차의 해싱 함수를 만들어낸다. 이 방법은 1차 군집에 취약하다.

2. Quandratic Probing

$h_i(x) = (h(x) + c_1i^2 + c_2i) \ \ mod \ \ m$

2차원 조사는 2차군집에 취약하다.

* 2차군집: 여러개의 원소가 동일한 초기 해쉬 함수값을 갖는 현상

3. Double Hashing

Double Hashing은 N차 해시 함수를 만들 때 전혀 다른 해시함수를 합쳐서 사용하는 방법이다.
$h_i(x) = (h(x) + if(x)) \ mod \ m$
![](https://i.imgur.com/QHCXH7S.png)
