# JPA 사용시 쿼리 바인딩 되는 값 확인하는 방법

```
logging.level.org.hibernate.SQL=debug 
```
이 경우는 SQL 문 로그는 보이지만 바인딩된 값이 보이지 않는다.
```
logging.level.org.hibernate.type.descriptor.sql=trace
```
위 설정까지 추가한다면 바인딩 되는 값을 확인할 수 있다.