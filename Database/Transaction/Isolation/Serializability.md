---
상위 개념: "[[Transaction Isolation]]"
---
# 직렬 가능성(Serializability)
직렬 실행은 항상 데이터베이스의 일관성을 유지한다고 가정해보자. 이 때 어떤 동시적인 스케쥴이 직렬 스케쥴의 결과와 동일하다면, 해당 스케쥴은 직렬 가능한(serializable) 스케쥴이라고 한다. 스케쥴의 동등성에는 다음과 같은 종류가 있다.
1. 충돌 직렬 가능성 (Conflict serializability)
2. 뷰 직렬 가능성 (View serializability)

## Conflict
서로 다른 트랜잭션에서 같은 데이터에 대해서 동시에 읽기만 하는 것은 충돌이 발생하지 않으며, 둘 중 적어도 하나의 트랜잭션에서 같은 데이터에 대해 쓰기를 하는 경우 충돌이 발생한다.