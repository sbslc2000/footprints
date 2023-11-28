# 스프링 시큐리티 의존성 추가 시
서버가 기동되면 스프링 시큐리티의 초기화 작업 및 보안 설정이 이루어진다. 
별도의 설정이나 구현을 하지 않아도 기본적인 웹 보안 기능이 현재 시스템에 연동되어 작동한다.

1. 모든 요청은 인증이 되어야 자원에 접근이 가능하다.
2. 인증 방식은 폼 로그인 방식과 httpBasic 로그인 방식을 제공한다.
3. 기본 로그인 페이지를 제공한다.
4. 기본 계정을 한 개 제공한다 : username: user, password: 랜덤 문자열 (콘솔에 출력)

이 상태로는 기본적인 기능만 제공되기 때문에, 계정 추가, 권한 추가, DB 연동 등 다양한 더 세부적이고 추가적인 보안 기능이 필요하다.

## 사용자 정의 보안 기능 구현

![](https://i.imgur.com/rkOUtiR.png)

스프링 시큐리티에 의존성을 추가하고 서버를 구동하면 여러 웹 보안 기능이 활성화된다. 하지만 기본 설정만으로는 기본계정 하나만이 존재하고, 이에 대해 별도로 추가된 권한이 존재하지 않는다. 또한 보안 위협에 대해서 방지하는 기능이 부재하다.

보안 기능을 구현하기 위해서는 사용자 정의 보안 내용들을 담은 설정 클래스를 만들어야 하며, 웹 보안 기능이 작동하게 설정하기 위해서는 설정 클래스가 **WebSecurityConfigurerAdapter**를 상속하게 해야한다. 

WebSecurityConfigurerAdapter은 **HttpSecurity**를 생성하며, HttpSecurity는 세부적인 보안 기능을 설정할 수 있는 API를 제공한다. HttpSecurity의 기능은 크게 인증 기능과 인가 기능을 부여하는 여러 API들을 제공한다.

## 설정 클래스
Spring Web MVC를 사용하면서 Security의 기능을 효과적으로 사용하기 위해서는 @EnableWebSecurity 애노테이션을 설정 클래스에 부착해야한다.
```java
@Configuration
@EnableWebSecurity 
public class SecurityConfig {
...
}
```

### @EnableWebSecurity 
```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Documented  
@Import({ WebSecurityConfiguration.class, SpringWebMvcImportSelector.class, OAuth2ImportSelector.class,  
       HttpSecurityConfiguration.class })  
@EnableGlobalAuthentication  
public @interface EnableWebSecurity {  
  
    /**  
     * Controls debugging support for Spring Security. Default is false.     * @return if true, enables debug support with Spring Security  
     */    boolean debug() default false;  
  
}
```

@EnableWebSecurity를 통해 4가지의 설정 클래스가 등록된다.
* **WebSecurityConfiguration.class** : 웹 관련 보안 기능을 지원하는 기술인 HttpSecurity, SecurityFilterChain 등의 정보를 구성한다.
* **SpringWebMvcImportSelector.class** : DispatcherServlet이 클래스패스에 존재하는 경우 WebMvcSecurityConfiguration을 등록한다.
* **OAuth2ImportSelector.class** : OAuth2와 관련된 구성정보를 등록한다.
* **HttpSecurityConfiguration.class** : HttpSecurity의 구성정보를 등록한다.

## HttpSecurity
#todo 

### 함께 보기
[[Authentication]]