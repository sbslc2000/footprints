---
공식 링크: "[[../MySQL|MySQL]]"
---
# MySQL Lock
MySQL에서 사용되는 잠금은 크게 Storage Engine Level과 MySQL Engine Level로 나눌 수 있다. MySQL Engine Level의 잠금은 모든 Storage Engine Level에 영향을 미치지만, Storage Engine Level의 잠금은 다른 Storage Engine에 영향을 미치지는 않는다.

MySQL Engine Level의 잠금으로는 테이블 락, 메타데이터 락, 네임드 락이 있다.
