---
상위 링크: "[[Spring Boot]]"
---
# @SpringBootApplication
@SpringBootApplication은 주로 스프링부트 애플리케이션의 Main 클래스에 붙어서 여러 설정 기능을 제공하는 역할을 한다.

```java
package org.springframework.boot.autoconfigure;  
  
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Inherited  
@SpringBootConfiguration  
@EnableAutoConfiguration  
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),  
       @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })  
public @interface SpringBootApplication {  
  
	@AliasFor(annotation = EnableAutoConfiguration.class)  
    Class<?>[] exclude() default {};  
  
    @AliasFor(annotation = EnableAutoConfiguration.class)  
    String[] excludeName() default {};  
  
    @AliasFor(annotation = ComponentScan.class, attribute = "basePackages")  
    String[] scanBasePackages() default {};  
  
    @AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")  
    Class<?>[] scanBasePackageClasses() default {};  
  
   @AliasFor(annotation = ComponentScan.class, attribute = "nameGenerator")  
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;  
  
    @AliasFor(annotation = Configuration.class)  
    boolean proxyBeanMethods() default true;  
  
}
```
