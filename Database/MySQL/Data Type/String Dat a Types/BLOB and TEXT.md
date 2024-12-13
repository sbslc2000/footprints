---
상위 개념: "[[String Data Types]]"
---
# BLOB
Binary Large OBject (BLOB)은 가변길이의 binary strings를 저장할 수 있는 타입이며, VARBINARY 보다 훨씬 큰 크기를 가질 수 있다.

TINYBLOB, BLOB, MEDIUMBLOB, LONGBLOB 이라는 BLOB 형태의 타입들이 있고 이들은 최대로 저장할 수 있는 값의 크기만이 다르다.

# TEXT 
TEXT는 가변길이의 non-binary strings를 저장할 수 있는 타입이며, VARCHAR보다 훨씬 큰 크기를 가질 수 있다.

TINYTEXT, TEXT, MEDIUMTEXT, LONGTEXT라는 TEXT 형태의 타입들이 있으며 이들은 최대로 저장할 수 있는 값의 크기만이 다르다.

# 제약 사항

## 정렬 
BLOB과 TEXT는 아주 큰 크기의 데이터를 다루기 위한 목적으로 사용되기 때문에 이들을 기준으로 정렬을 할 때에 일반적인 방법을 사용한다면 시스템에 큰 부담을 준다.

MySQL에는 MAX_SORT_LENGTH 라는 세션 변수가 있으며, BLOB이나 TEXT를 비교한다면 데이터의 처음부터 LENGTH 만큼의 byte만 비교의 대상이 된다.

## 임시 테이블
BLOB이나 TEXT를 다루는 임시 테이블은 메모리 공간이 아닌 DISK 공간에 존재하게 된다.

