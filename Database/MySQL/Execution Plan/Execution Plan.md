---
상위 링크: "[[MySQL]]"
---
# Execution Plan
옵티마이저가 쿼리를 실행하기 위해 수립한 계획. `EXPLAIN` 명령어로 확인할 수 있으며, 어떤 테이블을 어떤 순서로, 어떤 인덱스를 사용해, 어떤 방식으로 읽을지 보여준다.

튜닝의 출발점은 대부분 실행계획 해석이다. 느린 쿼리가 있을 때 실행계획을 먼저 확인하고, 비정상적인 접근 방식(풀스캔, filesort, 임시 테이블 등)이 있다면 그 원인을 제거하는 식으로 접근한다.

## EXPLAIN 주요 컬럼

| 컬럼              | 의미                                                                         |
| --------------- | -------------------------------------------------------------------------- |
| `id`            | SELECT 식별자. 서브쿼리/UNION 구조 파악에 사용                                           |
| `select_type`   | SELECT 종류 (SIMPLE, PRIMARY, SUBQUERY, DERIVED 등)                           |
| `table`         | 접근하는 테이블                                                                   |
| `type`          | 접근 방법. 좋은 순서대로 `system > const > eq_ref > ref > range > index > ALL`       |
| `possible_keys` | 사용 가능한 인덱스 후보                                                              |
| `key`           | 실제 선택된 인덱스                                                                 |
| `key_len`       | 사용된 인덱스 길이 (복합 인덱스에서 몇 컬럼까지 쓰였는지 추정)                                       |
| `ref`           | 인덱스 조회에 사용된 값의 출처                                                          |
| `rows`          | 읽을 것으로 예상되는 행 수                                                            |
| `filtered`      | WHERE 조건으로 남는 비율 (%)                                                       |
| `Extra`         | 추가 정보. `Using filesort`, `Using temporary`, `Using index`, `Using where` 등 |

## 주요 `Extra` 값

- `Using index` — 커버링 인덱스. 테이블 접근 없이 인덱스만으로 결과 생성. 좋음
- `Using where` — 스토리지 엔진이 넘긴 행을 MySQL 엔진이 추가로 필터링
- `Using filesort` — 인덱스 외 정렬 수행.
- `Using temporary` — 중간 결과를 임시 테이블로 구성. GROUP BY/DISTINCT에서 발생 가능
- `Using index condition` — ICP (Index Condition Pushdown) 적용

## 하위 문서

- [[Full Scan Query Pattern]]
- [[Using Filesort]]
