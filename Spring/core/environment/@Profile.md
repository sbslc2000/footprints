---
상위 링크: "[[Spring Core]]"
---
# @Profile 
만약 각 환경마다 서로 다른 빈*Bean*을 등록해야 한다면 어떻게 해야할까?

스프링은 @Profile 애노테이션을 통해 환경별로 빈 등록을 제어할 수 있다.

## 사용방법
```java
@Slf4j  
@Configuration  
public class PayConfig {  
  
    @Bean  
    @Profile("default")  
    public LocalPayClient localPayClient() {  
        log.info("localPayClient() called");  
        return new LocalPayClient();  
    }  
  
    @Bean  
    @Profile("prod")  
    public ProdPayClient prodPayClient() {  
        log.info("prodPayClient() called");  
        return new ProdPayClient();  
    }  
}
```
위 설정 클래스를 통해 기본 프로필에서는 LocalPayClient가, prod 환경에서는 ProdPayClient가 등록된다.

## 동작원리
Profile 애노테이션은 @Conditional() 애노테이션의 합성 애노테이션이다.
```java
@Target({ElementType.TYPE, ElementType.METHOD})  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Conditional(ProfileCondition.class)  
public @interface Profile {  
  
    /**  
     * The set of profiles for which the annotated component should be registered.     */    String[] value();  
  
}
```

ProfileCondition은 environment를 통해 프로필을 조회하고 주어진 value와 같은지를 검증하여, 빈 등록 여부를 결정한다.
```java
class ProfileCondition implements Condition {  
  
    @Override  
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {  
       MultiValueMap<String, Object> attrs = metadata.getAllAnnotationAttributes(Profile.class.getName());  
       if (attrs != null) {  
          for (Object value : attrs.get("value")) {  
             if (context.getEnvironment().acceptsProfiles(Profiles.of((String[]) value))) {  
                return true;  
             }  
          }  
          return false;  
       }  
       return true;  
    }  
  
}
```