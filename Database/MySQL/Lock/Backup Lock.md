---
상위 링크: "[[MySQL Lock]]"
---
# Backup Lock
MySQL 8버전에서 InnoDB 스토리지 엔진의 사용이 일반화가 되었고, InnoDB 스토리지 엔진은 트랜잭션을 지원하기 때문에 일관된 데이터 상태를 위해 모든 데이터 변경 작업을 멈출 필요가 없다. 따라서 조금 더 가벼운 수준의 글로벌 락으로써 백업 락이 도입되었다.

```sql
LOCK INSTANCE FOR BACKUP;

UNLOCK INSTANCE:
```

특정 세션에서 백업 락을 획득하면, 모든 세션에서 다음과 같은 행동이 제한된다.
* 데이터베이스 및 테이블 등 모든 객체 생성 및 변경, 삭제
* REPAIR RABLE과 OPTIMIZE TABLE 명령
* 사용자 관리 및 비밀번호 변경

허나 일반적인 데이터 변경은 허용된다.