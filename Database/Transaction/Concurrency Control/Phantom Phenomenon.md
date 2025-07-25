---
상위 개념: "[[Concurrency Control]]"
---
# Phantom Phenomenon
팬텀 현상(Phantom Phenomenon)은 하나의 트랜잭션이 특정 조건을 만족하는 데이터 집합을 쿼리할 때, 다른 트랜잭션이 해당 조건에 맞는 새로운 튜플을 삽입하거나 기존 튜플을 변경하여, 동일한 쿼리를 다시 시작했을 때 결과 집합의 개수가 달라지는 현상을 의미한다.

예를 들어, 트랜잭션 $T_1$이 'Physics' 학과 강사의 수를 세는 쿼리를 실행했다고 해보자. 이 때 트랜잭션 $T_2$가 이름이 'Feynman'인 새로운 'Physics' 학과 강사를 삽입하고 커밋했다. 이후에 트랜잭션 $T_1$이 강사 수를 세는 쿼리를 실행하면, 개수가 하나 더 많아진다. 

이 문제는 튜플 수준의 잠금(tuple-level locking)만으로는 탐지되지 않을 수 있다. 왜냐하면 충돌이 발생한 튜플은 트랜잭션이 쿼리를 처음 실행했을 때는 존재하지 않았거나 쿼리 조건에 해당하지 않았기 때문이다.