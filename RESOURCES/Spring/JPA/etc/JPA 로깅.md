# JPA 사용시 쿼리 바인딩 되는 값 확인하는 방법

## 쿼리 로깅

### hibernate의 콘솔 로깅
```yaml
spring:
	jpa:
		show-sql: true
```
이 방법은 콘솔 출력으로 로깅을 하기 때문에, 로그 파일로 생성하거나 로그 레벨을 조정하는 것이 불가능하다.

@DataJpaTest를 사용하면 이 설정이 자동으로 들어가며, 별도로 환경변수에 제공하여 false로 오버라이드하는 것이 불가능함을 확인했다.

###  로깅 프레임워크를 통한 로깅
```
logging.level.org.hibernate.SQL=debug 
```
Logback과 같은 로깅 프레임워크를 사용한 로깅으로, 로그의 출력 포맷이나 위치, 레벨 등을 상세하게 설정할 수 있다.

## 바인딩 값 로깅

### Hibernate 5 버전
쿼리를 로깅하더라도 SQL은 보이지만 바인딩된 값 ?로 나타난다.
```
logging.level.org.hibernate.type.descriptor.sql=trace
```
위 설정까지 추가한다면 바인딩 되는 값을 확인할 수 있다.

### hibernate 6 버전
```
logging.level.org.hibernate.orm.jdbc.bind=trace
```
6버전은 위 속성을 제공해야 바인딩 값을 확인할 수 있다.