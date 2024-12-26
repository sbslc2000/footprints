---
상위 링크: "[[Spring Core]]"
---
# @Configuration
```java
@Target({ElementType.TYPE})  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Component  
public @interface Configuration {  
    @AliasFor(  
        annotation = Component.class  
    )  
    String value() default "";  
  
    boolean proxyBeanMethods() default true;  
  
    boolean enforceUniqueMethods() default true;  
}
```

클래스 레벨의 애노테이션이며, 해당 클래스가 스프링 컨테이너에 등록될 빈의 정보를 담고 있음을 표기해주는 역할을 한다.