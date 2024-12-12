* [[JPA]]는 특정한 데이터베이스에 종속 되지 않아야 한다.
* 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다름
	* 가변 문자 : MySQL은 VARCHAR, Oracle은 VARCHAR2
	* 문자열을 자르는 함수 : SQL 표준은 SUBSTRING(), Oracle은 SUBSTR()
	* 페이징 : MySQL은 LIMIT, Oracle은 ROWNUM
* **방언*Dialect*** : SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능
![[Pasted image 20230921183507.png]]

Hibernate는 40가지 이상의 데이터베이스 방언을 지원하여 데이터베이스 이주를 편하게 할 수 있게 만들었다.
* hibernate.dialect 속성에 지정할 수 있음
