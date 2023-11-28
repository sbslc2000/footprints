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
@Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http.authorizeHttpRequests((authorize) -> authorize  
                        .anyRequest().authenticated()  
                ).formLogin(form ->  form  
                    .loginPage("/login.html")  
                    .defaultSuccessUrl("/")  
                    .successForwardUrl("/")  
                    .failureUrl("/login?error")  
                    .usernameParameter("username")  
                    .passwordParameter("password")  
                    .loginProcessingUrl("/login")  
                    .successHandler((request, response, authentication) -> log.info("login success"))  
                    .failureHandler((request, response, exception) -> log.info("login fail"))  
                    .permitAll()  
                );  
                
        return http.build();  
    }
```
* loginPage: 인증에 필요한 form을 담고 있는 페이지를 지정할 수 있다.
* defaultSuccessUrl : 로그인 후 이동할 url을 지정해준다. 로그인 요청 이후 클라이언트는 이 주소로 리다이렉트 된다.
* successForwardUrl : 사용이 제한적임. 추후 작성 예정 #todo
* failureUrl : 로그인 실패시 이동할 url을 지정해준다.
* usernameParameter : 로그인 시 ID에 해당하는 프로퍼티 명을 바꿔줄 수 있다.
* passwordParameter : 로그인 시 password에 해당하는 프로퍼티 명을 바꿔줄 수 있다.
* loginProcessingUrl : 로그인 로직을 수행하는 url을 지정해준다.
* successHandler : 로그인 성공 시 수행될 코드를 작성해줄 수 있다.
* failureHandler : 로그인 실패 시 수행될 코드를 작성해줄 수 있다.

### 주의할 점
체이닝을 통해 구성 정보를 제공할 때 우선순위가 있는 것 같다. 가령 다음과 같이 작성되어있다면
```java
http.authorizeHttpRequests((authorize) -> authorize  
                        .anyRequest().authenticated()  
                ).formLogin(form ->  form  
                    .defaultSuccessUrl("/")  
                    .successHandler((request, response, authentication) ->  {  
                        response.sendRedirect("/home");  
                        log.info("login success");  
                    })  
                    .permitAll()  
                );
```
로그인 성공시 defaultSuccessUrl 로 이동하지 않고 successHandler에서 제공한 리다이렉트 주소로 이동한다. 심지어 successHandler에 redirect 코드를 작성해주지 않아도 defaultSuccessUrl은 사용되지 않는다.
![](https://i.imgur.com/5mr6Hjl.png)

### loginPage()
loginPage에 파일명 속성을 제공하여 정적 파일 혹은 템플릿 엔진을 통해 커스텀 로그인 페이지를 띄울 수 있다.

```java
public SecurityFilterChain filterChain(HttpSecurity http) {
	http
		.formLogin(form -> form
			.loginPage("/login")
			.permitAll()
		);
	// ...
}
```

form에는 csrf 토큰이 들어가야 하는데, 이는 Thymeleaf에 의해서 자동으로 설정된다. 정적 파일로 수행했을 때는 이 토큰이 없어서 정상적으로 로그인이 처리되지 않음을 확인했다.
![](https://i.imgur.com/msw6eU0.png)

로그인 페이지를 타임리프를 통해 띄우고 싶은 경우 별도로 Controller에 사용될 url과 함께 매핑을 해주어야 한다.
```java
@GetMapping("/login")  
    public String login() {  
    return "login";  
}
```

### default 설정
FormLoginConfigurer을 사용하고 싶지 않은 경우 Customer.withDefaults()를 제공해주어 기본값으로 설정할 수 있다.
```java
@Bean  
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
    http.formLogin(Customizer.withDefaults());  
  
    return http.build();  
}
```

### permitAll()
permitAll()을 통해 failureUrl, loginPage, loginProcessingUrl과 같이 인증되지 않은 유저도 해당 url에 접근할 수 있게 해준다. boolean값을 파라미터로 받아서 허용할지 여부를 결정할 수 있으며 파라미터를 제공하지 않는다면 true가 기본값이다. 이것을 호출해주지 않으면 loginPage에 접근하기 위해 인증 과정을 거쳐야해서 다시 loginPage에 접근하게 되는 순환이 발생한다.
### 참조
[Form Login :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html)

