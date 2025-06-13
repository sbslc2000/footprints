---
상위 개념: "[[../JPA|JPA]]"
---
# JPA Auditing
JPA는 엔티티에 대해서
* 언제 생성되었고,
* 최근에 언제 수정되었으며,
* 누가 생성하였고,
* 최근에 누가 수정했는지
에 대한 정보를 기록할 수 있는 기능을 제공한다.

이 기능은 애노테이션 기반과 인터페이스 기반으로 구현할 수 있다.

## By Annotation
Jpa Auditing은 4가지 애노테이션을 제공한다.
* @CreatedDate
* @LastModifiedDate
* @CreatedBy
* @LastModifiedBy

## 활성화
Jpa Auditing을 활성화하기 위해서 구성정보를 등록해야한다. @EnableJpaAuditing 애노테이션은 시스템이 Auditing 기능을 활성화하게 만든다.
```java
@Configuration  
@EnableJpaAuditing  
public class AuditingConfig {  
  
  @Bean  
  public AuditorAware<AuditableUser> auditorProvider() {  
    return new AuditorAwareImpl();  
  }  
}
```
AuditorAware 구현체를 빈으로 등록하는 행위는 사용자 추적(CreatedBy, LastModifiedBy)를 사용할 때에는 필수이며, 시간을 추적하는 기능만 사용할 때에는 등록하지 않아도 된다.

Auditing 기능을 사용할 대상 엔티티의 클래스 레벨에는`@EntityListeners(AuditingEntityListener.class)` 애노테이션을 사용해야한다.
```java
@Entity  
@EntityListeners(AuditingEntityListener.class)  
public class Article {

	@CreatedDate
	private Instant createdDate;
	@LastModifiedDate
	private Instant lastModifiedDate;
	@CreatedBy
	private UserInfo createdBy;
	@LastModifiedDate
	private UserInfo lastModifiedBy;
}
```

## AuditorAware
> Interface for components that are aware of the application's current auditor. This will be some kind of user mostly.


AuditorAware 인터페이스는 다음 메서드를 갖는다.
```java
public interface AuditorAware<T> {  
    Optional<T> getCurrentAuditor();  
}
```
메서드의 파라미터에 별도로 값을 전달할 수 있으므로, 이 구현체에서는 ContextHolder와 같은 전역적으로 사용할 수 있는 객체로부터 현재 사용자 정보를 가져와야 한다.

제너릭 타입은 엔티티가 아닌 클래스이기만 하면 되며, 간단한 식별자 타입인 Long 혹은 String 부터 유저 정보를 담고 있는 타입 역시 가능하다. 다만 AuditorAware의 제너릭 타입과 엔티티의 @CreatedBy, @LastModifiedBy 필드 타입은 호환 가능해야한다.