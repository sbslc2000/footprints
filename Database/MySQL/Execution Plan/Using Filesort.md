---
상위 링크: "[[Execution Plan]]"
---
# Using Filesort
`EXPLAIN` 결과의 `Extra` 컬럼에 표시되는 값으로, **인덱스를 활용하지 못해 MySQL이 별도의 정렬 작업을 수행**한다는 의미다. 이름과 달리 반드시 디스크(file)를 사용하는 것은 아니며, 정렬 대상의 크기가 작으면 메모리 내에서 처리된다.

## 언제 발생하는가
`ORDER BY`, `GROUP BY`, `DISTINCT`, 윈도우 함수의 `OVER (ORDER BY ...)` 등에서 **요구되는 정렬 순서가 인덱스 순서와 일치하지 않을 때** 발생한다.

```sql
-- idx_created_at (created_at) 인덱스가 있다고 가정
SELECT * FROM orders ORDER BY created_at;              -- 인덱스 사용, filesort 없음
SELECT * FROM orders ORDER BY user_id;                 -- filesort 발생
SELECT * FROM orders ORDER BY created_at DESC, id ASC; -- ASC/DESC 혼합, filesort 발생 가능
SELECT * FROM orders WHERE status = 'P' ORDER BY created_at;
-- status 단일 인덱스만 있을 경우 filesort
```

즉, **정렬에 사용되는 컬럼이 인덱스 선두부터 순서대로 매칭되지 않으면** filesort가 발생한다.

## 동작 방식
MySQL은 두 가지 정렬 알고리즘 중 하나를 선택한다.

### 1. Original (Two-pass)
1. 정렬 키 + row ID만 읽어서 정렬
2. 정렬 결과 순서대로 테이블에서 나머지 컬럼 읽음

I/O가 한 번 더 발생하지만 정렬 버퍼는 적게 쓴다.

### 2. Modified (Single-pass, 기본값)
`SELECT` 대상 컬럼을 모두 포함한 레코드를 정렬 버퍼에 올려 한 번에 정렬. I/O는 1회지만 메모리 사용량이 크다.

### 메모리 vs 디스크
- 정렬 데이터가 `sort_buffer_size`에 다 들어가면 **메모리 정렬 (quicksort)**
- 버퍼를 넘치면 청크 단위로 정렬해 임시 파일로 쓰고, 최종적으로 **merge sort**
- `SHOW STATUS LIKE 'Sort%';`
    - `Sort_merge_passes` — merge 단계 발생 횟수. 이 값이 크면 디스크 정렬이 빈번한 것
    - `Sort_rows`, `Sort_scan`, `Sort_range` — 정렬된 행/방식별 카운트