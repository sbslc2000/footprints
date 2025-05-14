---
상위 개념: "[[Isolation/Concurrency Control Schemes|Concurrency Control Schemes]]"
---
# Schedules
스케쥴이란 동시 트랜잭션의 명령이 실행되는 시간 순서를 지정하는 명령의 시퀀스이다.

트랜잭션 세트에 대한 스케쥴은 해당 트랜잭션의 모든 명령어를 포함하고 있어야 하며, 각 개별 트랜잭션에서의 명령어 순서는 유지되어야 한다.

## Example
$T_1$은 A에서 B로 $50 을 송금하고, $T_2$는 잔액의 10%를 A에서 B로 송금한다.

### $T_1$ 이후에 $T_2$를 수행하는 순차 스케쥴
![](https://i.imgur.com/iP8h4Uo.png)

