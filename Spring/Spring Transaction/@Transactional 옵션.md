---
상위 링크: "[[@Transactional]]"
---
# 트랜잭션 옵션
```java
@Target({ElementType.TYPE, ElementType.METHOD})  
@Retention(RetentionPolicy.RUNTIME)  
@Inherited  
@Documented  
@Reflective  
public @interface Transactional {  
    @AliasFor("transactionManager")  
    String value() default "";  
  
    @AliasFor("value")  
    String transactionManager() default "";  
    String[] label() default {};  
    Propagation propagation() default Propagation.REQUIRED;  
    Isolation isolation() default Isolation.DEFAULT;  
    int timeout() default -1;  
    String timeoutString() default "";  
    boolean readOnly() default false;  
    Class<? extends Throwable>[] rollbackFor() default {};  
    String[] rollbackForClassName() default {};  
    Class<? extends Throwable>[] noRollbackFor() default {};  
    String[] noRollbackForClassName() default {};  
}
```
@Transactional 애노테이션은 위와 같은 엘리먼트를 갖고 있다.

### value, transactionManager
트랜잭션에 필요한 트랜잭션 매니저를 직접 설정할 수 있다. 이 값은 제공하지 않는다면 스프링이 기본으로 등록된 트랜잭션 매니저를 주입해준다.

### rollbackFor
스프링 트랜잭션은 언체크 예외의 경우 롤백을 수행하고, 체크 예외의 경우 커밋을 수행한다. rollbackFor에 예외 클래스를 지정하면 해당 예외가 발생했을 때에는 롤백을 수행할 수 있도록 설정을 오버라이드 할 수 있다.

noRollbackFor은 rollbackFor의 반대 역할을 한다.

### propagation
[[트랜잭션 전파]]
### isolation
[트랜잭션 격리 수준](ACID.md##Isolation%20Level)을 지정할 수 있다. 기본 값은 DEFAULT로 데이터베이스에서 설정한 기준을 따른다. 
### timeout
트랜잭션 수행 시간에 대한 타임아웃을 초 단위로 지정한다. 기본 값으로는 트랜잭션 시스템의 타임아웃을 사용한다. 운영 환경에 따라 동작하지 않는 경우도 있다.

timeoutString을 통해 숫자 대신 문자로 타임아웃 값을 지정할 수 있다.

### label
트랜잭션 애노테이션에 있는 라벨 값을 읽어서 어떤 동작을 추가로 부여할 때 사용할 수 있다.

### readonly
기본적으로는 false, 즉 읽기 쓰기가 모두 가능한 트랜잭션으로 생성되지만, 쓰기가 사용되지 않는 트랜잭션의 경우 성능 최적화를 위해 true 값을 지정해줄 수 있다. 
* **프레임워크**
	* JdbcTemplate은 읽기 전용 트랜잭션에서 변경 쿼리가 발생한다면 예외를 던진다
	* JPA는 읽기 전용 트랜잭션에서 커밋 시점에 플러시를 호출하지 않는다. 또한 변경 감지를 위한 스냅샷 객체 역시 생성하지 않는다.
* **JDBC 드라이버**
	* 이 내용들은 DB와 드라이버 버전에 따라 다르게 동작하기 때문에 환경에 따라 확인이 필요하다.
	* 읽기 전용 트랜잭션에서 변경 쿼리가 발생하면 예외를 던진다.
	* Replication이 되어있는 경우 읽기 전용일 때에는 slave DB에 쿼리를 날린다.
* **데이터베이스**
	* DBMS에 따라서 읽기 전용 트랜잭션을 생성한다면 내부적으로 성능 최적화가 발생하기도 한다.