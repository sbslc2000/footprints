---
상위 개념: "[[MySQL Engine]]"
---
# 메모리 할당 및 사용 구조

MySQL에서 사용되는 메모리 공간은 크게 글로벌 메모리 영역과 로컬 메모리 영역으로 구분된다.

# 글로벌 메모리 영역
MySQL 스레드들에 의해 공유되는 메모리 영역으로 다음과 같은 내용을 담고 있다.
* 테이블 캐시
* InnoDB Buffer Pool
* InnoDB Adaptive Hash Index
* InnoDB Redo Log Buffer

# 로컬 메모리 영역
세션 메모리 영역이라고도 표현하며, 클라이언트 스레드가 쿼리를 처리하는데 사용하는 메모리 영역이다. 각 클라이언트 스레드별로 독립적으로 할당되며 절대 공유되지 않는다.

* Sort buffer
* Join buffer
* Binary Log Cache
* Network Buffer