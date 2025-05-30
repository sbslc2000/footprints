---
상위 개념: "[[../Schedule/Schedule]]"
---
# Cascadeless Schedule

> 비연쇄적인 스케줄은 모든 트랜잭션 쌍 $T_i$, $T_j$에 대해, $T_i$에 의해 기록된 데이터를 $T_j$가 읽기 연산을 실행하기 전에 $T_i$d의 커밋 연산이 먼저 실행되는 스케줄을 말한다.

아무리 스케줄이 복구 가능하다고 하더라도, 상황에 따라 여러 트랜잭션을 되돌려야 할 수도 있다.
![](https://i.imgur.com/sLXXDq7.png)

위 스케줄에서 $T_{10}$은 $T_9$에 종속적이며, $T_9$는 $T_8$에 종속적이다. 따라서 $T_8$의 실패는 $T_9$와 $T_{10}$의 롤백을 야기한다.

이렇게 하나의 트랜잭션의 취소로 인해 다른 일련의 트랜잭션들이 롤백되는 현상을 **연쇄적 롤백**(cascading rollback)이라고 한다.

연쇄적 롤백은 상당히 많은 양의 작업을 취소시키기 때문에 바람직하지 않다. 따라서 연쇄적 롤백이 발생하지 않도록 스케줄에 제한을 주어야 하며, 이러한 스케줄을 **비연쇄적인 스케줄**(cascadeless schedule)이라고 한다.

모든 비연쇄적인 스케줄은 [복구 가능](../Schedule/Recoverable%20Schedule.md)하다.