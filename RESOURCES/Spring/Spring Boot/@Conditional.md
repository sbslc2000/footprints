---
상위 링크: "[[Auto Configuration]]"
---
# @Conditional
## Condition
```java
package org.springframework.context.annotation;

/**  
 * A single {@code condition} that must be {@linkplain #matches matched} in order  
 * for a component to be registered. 
 * 
 * <p>Conditions are checked immediately before the bean-definition is due to be 
 * registered and are free to veto registration based on any criteria that can 
 * be determined at that point. 
 * 
 * <p>Conditions must follow the same restrictions as {@link BeanFactoryPostProcessor}  
 * and take care to never interact with bean instances. For more fine-grained control 
 * of conditions that interact with {@code @Configuration} beans consider implementing  
 * the {@link ConfigurationCondition} interface.  
 * 
 * @author Phillip Webb  
 * @since 4.0  
 * @see ConfigurationCondition  
 * @see Conditional  
 * @see ConditionContext  
 */
 @FunctionalInterface  
public interface Condition {  
  
    /**  
     * Determine if the condition matches.     
     * @param context the condition context  
     * @param metadata the metadata of the {@link org.springframework.core.type.AnnotationMetadata class}  
     * or {@link org.springframework.core.type.MethodMetadata method} being checked  
     * @return {@code true} if the condition matches and the component can be registered,  
     * or {@code false} to veto the annotated component's registration  
     */    
     boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);  
  
}
```

Condition 인터페이스는 matches()를 가지고 있는 인터페이스이다. 이는 @Conditional을 통해 사용되며, 컴포넌트의 빈 등록 여부를 결정한다.

- matches() : 조건을 만족하는지 여부를 반환한다.
- ConditionContext: 스프링 컨테이너와 환경 정보등을 담고 있다.
- AnnotatedTypeMetadata : 애노테이션 메타 정보를 담고 있다.

```java
//예시 1, Property를 읽어서 조건을 만족하는지 여부를 반환하는 Condition 구현체
public class MemoryCondition implements Condition {  
  
    @Override  
    public boolean matches(
	    ConditionContext context,
	    AnnotatedTypeMetadata metadata
	) {  
        String memory = context.getEnvironment().getProperty("memory");  
        return "on".equals(memory);  
    }  
}
```

## @Conditional

```java
@Target({ElementType.TYPE, ElementType.METHOD})  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
public @interface Conditional {  
  
    /**  
     * All {@link Condition} classes that must {@linkplain Condition#matches match}  
     * in order for the component to be registered.     
     */    
	Class<? extends Condition>[] value();  
  
}
```

@Conditional 애노테이션은 스프링 설정 클래스와 함께 사용되어, 설정 클래스 내의 빈 팩토리 메서드를 실행할 지 말지를 결정한다.

```java
//예시. MemoryCondition을 사용하는 @Conditional 구성 클래스
//이 경우 Condition이 만족하는 경우만 빈이 등록된다.
@Configuration  
@Conditional(MemoryCondition.class)  
public class MemoryConfig {  
  
    @Bean  
    public MemoryController memoryController() {  
        return new MemoryController(memoryFinder());  
    }  
  
    @Bean  
    public MemoryFinder memoryFinder() {  
        return new MemoryFinder();  
    }  
}
```

## @Conditional 합성 애노테이션

@Conditional은 스프링에서 제공하는 애노테이션이며, 스프링부트에서는 이를 더 추상화하여 편리하게 이용할 수 있게 만들어주는 @ConditionalOnXXX 애노테이션을 제공한다.

- **@ConditionalOnProperty**

```java
//ConditionalOnProperty 예제. memory=on 이라는 환경변수가 있다면 빈을 등록한다.  
@Configuration  
@ConditionalOnProperty(name = "memory", havingValue = "on")  
public class MemoryConfig {  
  
    @Bean  
    public MemoryController memoryController() {  
        return new MemoryController(memoryFinder());  
    }  
  
    @Bean  
    public MemoryFinder memoryFinder() {  
        return new MemoryFinder();  
    }  
}
```

@ConditionalOnProperty는 내부적으로 @Conditional 애노테이션을 가지고 있다. OnPropertyCondition은 환경변수와 주어진 값을 체크해주는 역할을 하는 컨디션 클래스이며, 스프링부트에서 제공한다.

```java
@Retention(RetentionPolicy.RUNTIME)  
@Target({ElementType.TYPE, ElementType.METHOD})  
@Documented  
@Conditional({OnPropertyCondition.class})  
public @interface ConditionalOnProperty {  
    String[] value() default {};  
  
    String prefix() default "";  
  
    String[] name() default {};  
  
    String havingValue() default "";  
  
    boolean matchIfMissing() default false;  
}
```

추가적인 애노테이션들은 다음과 같다.

- @ConditionalOnClass, @ConditionalOnMissingClass
    - 클래스가 있는 경우 동작하거나, 없는 경우 동작한다.
- @ConditionalOnBean, @ConditionalOnMissingBean
    - 빈이 등록되어 있는 경우 동작하거나, 없는 경우 동작한다.
- @ConditionalOnResource
    - 리소스가 있는 경우 동작한다.
- @ConditionalOnWebApplication, @ConditionalOnNotWebApplication
    - 웹 애플리케이션인 경우 동작한다.
- @ConditionalOnExpression
    - SpEL 표현식에 만족하는 경우 동작한다.

[Core Features](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.developing-auto-configuration.condition-annotations)