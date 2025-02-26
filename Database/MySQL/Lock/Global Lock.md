---
공식 링크: "[[MySQL Lock]]"
---
# Global Lock
Global Lock은 `FLUSH TABLES WITH READ LOCK` 명령으로 획득할 수 있으며, MySQL에서 제공하는 잠금 중 가장 범위가 크다.

한 세션에서 Global Lock을 획득하면, 다른 세션에서 SELECT를 제외한 대부분의 DDL이나 DML의 실행은 락이 해제될 때까지 대기 상태로 남는다. Global Lock이 영향을 미치는 범위는 MySQL 서버 전체이며, 작업 대상 **테이블**이나 **데이터베이스**가 다르더라도 동일하게 영향을 미친다.

만약 명령이 처리되기 직전에 다른 쿼리가 특정 자원에 대한 잠금을 갖고 있었다면, 해당 쿼리가 끝난 이후 명령이 수행된다.