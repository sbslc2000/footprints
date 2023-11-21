# JpaRepository 빈 등록
```java
public interface PostRepository extends JpaRepository<Post,Long> {
}
```

스프링 데이터 JPA를 이용하면 JpaRepository 인터페이스를 상속한 인터페이스만 만들어도 DAO 역할을 하는 객체가 스프링 빈으로 등록된다. 어떠한 원리를 통해 이런 일이 생기는 것일까?

## @EnableJpaRepositories
```java
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Import(JpaRepositoriesRegistrar.class)  
public @interface EnableJpaRepositories {
...
}
```
단순 스프링만 이용하면 JpaRepository 빈을 등록하기 위해서는 @EnableJpaRepositories 애노테이션을 Configuration 클래스에 작성해줘야 한다.  스프링 부트의 경우 자동으로 해당 구성정보를 설정해준다.

## JpaRepositoriesRegistrar
JpaRepositoriesRegistrar 클래스는 스프링이 지원하는 ImportBeanDefinitionRegistrar을 구현한 클래스이다. ImportBeanDefinitionRegistrar은 런타임에 프로그래밍 방식으로 객체를 만들고 빈을 등록해주는 기능을 한다.

### ImportBeanDefinitionRegistrar
ImportBeanDefinitionRegistrar를 구현한 뒤 @Import 애노테이션을 사용하여 스프링 빈 구성 시점에 빈을 등록할 수 있다.
```java
@Data  
static class Hello {  
    private String name;  
}

static class HelloRegistrar implements ImportBeanDefinitionRegistrar {  
    @Override  
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {  
        GenericBeanDefinition beanDefinition = new GenericBeanDefinition();  
        beanDefinition.setBeanClass(Hello.class);  
        beanDefinition.getPropertyValues().add("name", "goorm");  
        registry.registerBeanDefinition("hello", beanDefinition);  
    }  
}
```
HelloRegistrar은 ImportBeanDefinitionRegistrar을 구현한다. BeanDefinition을 정의하기 위한 많은 클래스들이 있으며, 이를 사용해서 프로그래밍적으로 만들어질 빈의 구성정보를 작성할 수 있다.
```java
@TestConfiguration  
@Import(HelloRegistrar.class)  
static class Config {  
  
}

@Test  
public void test() {  
    Hello bean = ac.getBean(Hello.class);  
    assertThat(bean.getName()).isEqualTo("goorm");  
}
```
이후 Configuration 파일에 Registrar을 Import 해주면 스프링 빈을 초기화하는 시점에 정보로 활용하여 빈을 생성한다.
![](https://i.imgur.com/461XoRV.png)

결국 JpaRepository를 상속한 인터페이스를 만드는 과정은 ImportBeanDefinitionRegistrar에서 동적으로 리플렉션 기술을 사용하여 복잡한 빈을 만들어 내는 과정이다. 