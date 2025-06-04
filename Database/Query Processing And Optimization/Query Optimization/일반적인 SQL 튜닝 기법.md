---
상위 개념: "[[Query Optimization]]"
---
# 일반적인 SQL 튜닝 기법

* 바인드 변수를 사용
	* 상수 값이 바뀌더라도 이전 질의를 동일하게 사용함으로써, 최적화 실행 단계를 줄일 수 있다.
* 가급적 WHERE 조건에서 인덱스 컬럼을 모두 사용
* 인덱스 컬럼에 사용하는 연산자는 가급적 동등을 사용
* 인덱스 컬럼은 변형을 하여 사용하지 않음
	* ex. `substr(last_name, 1, 1) = '오';`  -> `last_name like '오%'`
* OR 보다는 AND를 사용
* 그룹핑 쿼리는 가급적 HAVING 보다는 WHERE 절에서 데이터를 필터링
* DISTINCT는 가급적 사용하지 않음
* IN, NOT IN 대신에 EXISTS와 NOT EXISTS를 사용
* 