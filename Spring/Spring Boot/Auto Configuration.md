---
상위 링크: "[[Spring Boot]]"
---
# Auto Configuration
스프링부트는 자동 구성 *Auto Configuration*이라는 기능을 제공하여, 일반적으로 자주 사용하는 수 많은 빈 들을 자동으로 등록해준다. 자동 구성을 통해 개발자는 반복적이고 복잡한 빈 등록과 설정을 최소화하고 애플리케이션 개발을 빠르게 시작할 수 있다.

스프링부트는 spring-boot-autoconfigure 라는 프로젝트 안에서 수 많은 자동 구성을 제공한다.

스프링부트가 제공하는 자동 구성 기능을 이해하려면 다음 개념을 이해해야 한다.
1. @Conditional : 특정 조건에 맞을 때 설정이 동작하도록 한다.
2. @AutoConfiguration : 자동 구성이 어떻게 동작하는지 내부 원리 이해

## @EnableAutoConfiguration
@EnableAutoConfiguration은 @SpringBootApplication에 붙어있는 애노테이션으로, [[ImportSelector]]를 구현한 AutoConfigurationImportSelector를 통해 스프링 구성 정보를 세팅한다. AutoConfigurationImportSelector가 각 라이브러리의 자동 구성 정보를 읽어내어 등록하는 역할을 한다.
```java
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Inherited  
@AutoConfigurationPackage  
@Import(AutoConfigurationImportSelector.class)  
public @interface EnableAutoConfiguration {  
  
	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";  
  
    Class<?>[] exclude() default {};  
  
    String[] excludeName() default {};  
  
}
```

## @AutoConfiguration
@AutoConfiguration은 @Configuration의 합성 애노테이션으로 자동 구성 정보를 담는다.
```java
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Configuration(proxyBeanMethods = false)  
@AutoConfigureBefore  
@AutoConfigureAfter  
public @interface AutoConfiguration { }
```

AutoConfiguration 설정 클래스는 일반 컴포넌트 스캔 방식의 설정 클래스와 라이프사이클이 다르므로 컴포넌트 스캔의 대상이 되어서는 안된다. @SpringBootApplication에는 AutoConfigurationExcludeFilter를 통해 자동 구성 클래스들이 컴포넌트 스캔의 대상이 되지 않게 한다.
```java
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),  
       @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

## 자동 구성은 언제 사용하는가?
Auto Configuration은 라이브러리를 직접 만들어서 제공할 때 사용하고, 그 외에는 사용할 일이 거의 없다. 

개발을 하다 보면 사용하는 빈들이 어떻게 등록된 것인지 확인이 필요할 때가 있다. 이럴 때 스프링부트의 자동 구성 코드를 읽고 이해할 수 있어야 한다. 자동화는 매우 편리하지만, 실무에서 문제가 발생했을 때 대처하는 법을 모른다면 큰 코 다치기 쉽다.