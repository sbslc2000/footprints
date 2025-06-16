---
상위 개념: "[[Concurrency Control|Concurrency Control]]"
---
# Non-repeatable Read
반복 불가능한 읽기(Non-repeatable read)는 하나의 트랜잭션 내에서 같은 데이터 항목을 두 번 이상 읽을 때, 그 값이 달라지는 현상을 의미한다.

만약 트랜잭션 $T_1$이 데이터 항목 Q를 읽은 후, 트랜잭션 $T_2$가 데이터 항목 Q를 변경하고 커밋하는 경우에 발생한다. 이후 $T_1$이 다시 A를 읽으면, 첫 번째 읽은 값과 두 번째 읽은 값이 달라진다.