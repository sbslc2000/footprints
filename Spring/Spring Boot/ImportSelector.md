---
상위 링크: "[[Auto Configuration]]"
---
# ImportSelector
## @Import
@Import는 설정 정보를 추가하는 기능을 해주는 애노테이션이다. 컴포넌트 스캔 방식으로 등록되지 않는 설정 클래스를 등록할 수 있다.

@Import 는 2가지 방법을 통해 설정 클래스를 등록할 수 있다.
1. 정적인 방법 : 설정 클래스를 직접 코드에 추가하는 방법
2. 동적인 방법 : 프로그래밍 방식으로 설정 클래스를 결정하는 방법

```java
// 1. 정적인 방법 예시
@Configuration
@Import(HelloConfig.class)
public MyAppConfig { ... }
```

## ImportSelector
스프링은 설정 정보 대상을 동적으로 선택할 수 있는 ImportSelector 인터페이스를 제공한다. ImportSelector의 반환값은 설정 클래스의 식별자로, @Import에 ImportSelector가 있는 경우 반환값의 설정 클래스에 대한 구성을 수행한다. 스프링부트는 이 인터페이스를 구현하여 파일에 적혀있는 자동 구성 클래스들의 패키지를 읽어내어 반환한다.

```java
// ImportSelector 구현체 예시
public class HelloImportSelector implements ImportSelector {  
    @Override  
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {  
	    //이 부분은 프로그래밍에 의해서 동적으로 바뀔 수 있다.
        return new String[]{"hello.selector.HelloConfig"};  
    }  
}
```

```java
// @Import와 ImportSelector 테스트용 코드
class ImportSelectorTest {  
  
    @Test  
    void staticConfig() {  
        AnnotationConfigApplicationContext appContext = new AnnotationConfigApplicationContext(StaticConfig.class);  
        HelloBean helloBean = appContext.getBean(HelloBean.class);  
  
        assertThat(helloBean).isNotNull();  
    }  
  
    @Test  
    void selectorConfig() {  
        AnnotationConfigApplicationContext appContext = new AnnotationConfigApplicationContext(SelectorConfig.class);  
  
        HelloBean helloBean = appContext.getBean(HelloBean.class);  
        assertThat(helloBean).isNotNull();  
    }  
  
    @Configuration  
    @Import(HelloConfig.class)  
    public static class StaticConfig {  
    }  
  
    @Configuration  
    @Import(HelloImportSelector.class)  
    public static class SelectorConfig {  
    }  
}
```
