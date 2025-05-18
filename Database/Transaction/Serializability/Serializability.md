---
상위 개념: "[[Transaction Isolation]]"
---
# Serializability
직렬 실행은 항상 데이터베이스의 일관성을 유지한다고 가정해보자. 이 때 어떤 동시적인 스케쥴이 직렬 스케쥴의 결과와 동일하다면, 해당 스케쥴은 직렬 가능한(serializable) 스케쥴이라고 한다. 스케쥴의 동등성에는 다음과 같은 종류가 있다.
1. 충돌 직렬 가능성 (Conflict serializability)
2. 뷰 직렬 가능성 (View serializability)

