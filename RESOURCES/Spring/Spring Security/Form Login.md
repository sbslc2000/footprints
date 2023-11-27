# Form Login
Spring Security는 HTML form을 통해 username과 password를 제공받는 인증 방식을 제공한다.
![](https://i.imgur.com/JqKrBWf.png)

1. 사용자는 인증이 필요한 자원에 요청한다.
2. Spring Security의 `AuthorizationFilter`에서 인증되지 않은 요청은 거부하고 `AccessDeniedException`을 발생시킨다.
3. `ExceptionTranslationFilter`에서 AuthenticationEntryPoint에 지정된 login page로 리다이렉트시킨다. 
4. 사용자는 로그인 페이지를 요청한다.
5. 서버는 로그인 페이지를 반환한다.

username과 password가 서버로 전달될 때, `UsernamePasswordAuthenticationFilter`가 아이디와 비밀번호의 유효성을 검증한다. 

![](https://i.imgur.com/Z9KxOCe.png)

1. 사용자가 제출한 username과 password을 통해 `UsernamePasswordAuthenticationFilter`에서  `UsernamePasswordAuthenticationToken`을 생성한다. 이 작업은 HttpRequest에서 username과 password를 추출하는 방식으로 동작한다.
2. `UsernamePasswordAuthenticationToken`은 인증절차를 거치기 위해 `AuthenticationManager`로  전달된다. AuthenticationManager는 유저 정보가 저장되어있는 방식에 따라 변경되어야 한다.
3. 인증에 실패하면, 다음과 같은 과정을 거친다.
	1. `SecurityContextHolder` 가 비워진다.
	2. `RememberMeServices`의 loginFail이 수행된다.
	3. `AuthenticationFailureHandler` 가 수행된다.
4. 인증에 성공하면, 다음과 같은 과정을 거친다.
	1. `SessionAuthenticationStrategy`에게 새로운 로그인이 발생했음을 알린다.
	2. `RememberMeServices`의 loginSuccess가 수행된다. (optional)
	3. `ApplicationEventPublisher`가 `InteractiveAuthenticationSuccessEvent`를 발생시킨다.
	4. `AuthenticationSuccessHandler`가 수행된다.

Form Login은 스프링 시큐리티에 의해 기본적으로 설정된다. 

## SecurityFilterChain 설정
```java
@Bean  
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
    http.authorizeHttpRequests((authorize) -> authorize  
                    .anyRequest().authenticated()  
            ).formLogin(Customizer.withDefaults());  
  
    return http.build();  
}
```

authorizationHttpRequests 이후 체이닝 메서드를 통해 formLogin()을 호출하면 form login 방식의 인증이 적용된다. 아무런 작성을 하지 않아도 기본값으로 작동하긴 한다.

## FormLoginConfigurer
Form Login과 관련된 설정을 추가적으로 제공하기 위해서 FormLoginConfigurer을 사용할 수 있다. FormLoginConfigurer은 formLogin의 메서드로 제공하여 적용시킬 수 있다.
```java
http.authorizeHttpRequests((authorize) -> authorize  
                .anyRequest().authenticated()  
        ).formLogin(configurer -> {  
            configurer.loginPage("/login");  
            configurer.defaultSuccessUrl("/home");  
            configurer.failureUrl("/login?error");  
            configurer.usernameParameter("username");  
            configurer.passwordParameter("password");  
            configurer.loginProcessingUrl("/login");  
            configurer.successHandler((request, response, authentication) -> log.info("login success"));  
            configurer.failureHandler((request, response, exception) -> log.info("login fail"));  
            configurer.permitAll();  
        });
```


[Form Login :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html)

