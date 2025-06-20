---
상위 개념: "[[Log-based Recovery]]"
---

# Log Record
로그(Log)는 데이터베이스의 변경 사항을 기록하기 위해 가장 널리 사용되는 자료구조이다. 로그는 연속된 로그 레코드(log record)로 구성되며, 데이터베이스의 모든 갱신 작업을 기록한다.

## Update Log Record
갱신 로그 레코드(Update Log Record)는 하나의 데이터베이스 쓰기 연산을 기술하며, 다음과 같은 필드를 갖는다.

* 트랜잭션 식별자(transaction identifier)($T_i$): 쓰기 연산을 수행한 트랜잭션의 고유 식별자
* 데이터 항목 십결자(data-item identifier)($X_j$): 트랜잭션이 쓰기 연산을 수행한 데이터 항목의 고유 식별자이다. 일반적으로 데이터 항목의 디스크 상 위치를 사용하며, 해당 데이터 항목이 들어있는 블록의 블록 식별자와 블록 내의 오프셋으로 구성된다.
* 이전 값(old value)($V_1$): 쓰기 연산을 실행하기 전 데이터 항목의 값이다.
* 새로운 값(new value)($V_2$): 쓰기 연산을 실행한 이후 데이터 항목이 가질 값이다.

이를 <$T_i$, $X_j$, $V_1$, $V_2$> 로 나타내기도 한다.

## 특별한 로그 레코드

* <$T_i$, start> : 트랜잭션 $T_i$가 시작되었음을 의미
* <$T_i$ commit>: 트랜잭션 $T_i$가 커밋했음을 의미
* <$T_i$ abort>  : 트랜잭션 $T_i$가 중단되었음을 의미