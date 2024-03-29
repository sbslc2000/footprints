---
상위 개념: "[[Authentication]]"
---
# Anonymous Authenitcation
일반적으로 'deny-by-default' 전략을 사용하여 따로 접근 가능하게 하는 자원에 대해 명시적으로 지정하고 나머지는 제한하는 것이 좋은 패턴이다. 인증되지 않은 유저에게 접근 권한을 허용해야하는 페이지는 login , logout , homepage 등이 있을 수 있는데, 이들에 대한 접근을 처리하는 방법에는 여러가지가 있다.
첫번째 방법은 이러한 요청들에 대해 어떠한 보안조치도 수행하지 않는 것이다. 이 요청들에 대해서 filter chain을 skip하는 방식으로 구현될 수 있다. 하지만 이러한 방식은 해당 페이지에서 인증된 유저에 대해서는 특정한 로직을 수행해야하는 경우 바람직하지 않다. (예를 들어 인증된 유저는 로그아웃을 하지 않는 다면 로그인 페이지가 보이지 않아야함)

이런 문제를 해결하기 위해서 모든 사용자에게 어떠한 기본적인 ROLE을 부여하고, 해당 ROLE을 가진 사람에게는 login 페이지 등 기본적인 페이지에 접근할 수 있게 하는 방법을 사용할 수 있다. 이러한 방법은 **Anonymous Authentication**이라고 부른다. Anonymous Authentication의 제공으로 스프링 시큐리티는 접근 제어를 할 수 있는 더 편리한 방법을 제공한다.

## Configuration 및 기본 사용법

Anonymous Authentication의 주요한 기능은 AnonymousAuthenticationFilter에서 수행된다.
![](https://i.imgur.com/OlYz5Vs.png)

```java
@Configuration  
@EnableWebSecurity  
public class AnonymousAuthenticationConfig {  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http.authorizeHttpRequests((authorize) -> authorize  
                        .requestMatchers("/anonymous").anonymous()  
                        .requestMatchers("/").permitAll()  
                        .anyRequest().authenticated()  
                ).formLogin(Customizer.withDefaults())  
                .anonymous(Customizer.withDefaults());  
  
        return http.build();  
    }  
}
```

위 설정과 같이 작성해주면 /anonymous 요청에 대해서는 로그인 하지 않고도 접근할 수 있다. 그렇다면 permitAll() 설정을 해준 요청들과는 어떤 점에서 차이가 날까?

permitAll() 속성은 말 그대로 모든 요청에 대한 승인으로, 사용자의 인증여부와 상관없이 해당 자원을 반환한다. 반면 anonymous() 속성은 오직 익명 인증 상태에서만 허용되므로, 로그인한 사용자는 해당 자원들에 접근할 수 없다.

### AnonymousConfigurer
filter chain에서 anonymous() 메서드에 AnonymousConfigurer를 제공하여 기본 구성 정보를 변경할 수 있다.

익명 Authentication 객체의 principal 이름을 변경하고 싶으면 configurer의 principal 메서드에 변경하고자 하는 이름을 지정해줄 수 있다. 기본값은 anonymousUser이다.
```java
http.authorizeHttpRequests((authorize) -> authorize  
                .requestMatchers("/anonymous").anonymous()  
                .requestMatchers("/").permitAll()  
                .anyRequest().authenticated()  
        ).formLogin(Customizer.withDefaults())  
        .anonymous(configurer -> configurer  
                .principal("anonymous"));
```

![](https://i.imgur.com/BwOkGZU.png)

authorities 메서드를 사용하면 익명 Authentication 객체의 권한을 변경해줄 수 있다.
```java
.anonymous(configurer -> configurer  
        .principal("anonymous")  
        .authorities("ROLE_ANONYMOUS"));
```

## AuthenticationTrustResolver
AuthenticationTrustResolver는 isAnonymous(Authentication) 이라는 메서드를 제공하는 인터페이스이다. 이를 통해 Authentication 객체가 익명인지 확인하고자 하는 클래스들은 객체의 익명 여부를 확인할 수 있다.

이것이 사용되는 대표적인 예시는 ExceptionTranslationFilter에서 AccessDeniedException을 처리할 때이다. 익명 Authentication 사용자로부터 AccessDeniedException이 발생한다면, 403 포비든을 제공하는 것 대신에 login 화면으로 이동시켜야할 것이다. 하지만 Authentication이 null이 아니고 익명 객체가 존재하는 상황이기에 login form 등에 접근이 제한되는 상황이 발생할 수 있다. 이러한 문제를 해결하기 위해 AuthenticationTrustResolver가 사용된다.
```java
//ExceptionTranslationFilter.java 
private void handleAccessDeniedException(...) throws ServletException, IOException {  
    Authentication authentication = 
    this.securityContextHolderStrategy
    .getContext()
    .getAuthentication();  
    
    boolean isAnonymous = this.authenticationTrustResolver
    .isAnonymous(authentication);  
    
    if (isAnonymous || 
    this.authenticationTrustResolver
    .isRememberMe(authentication)
    ) {  
       sendStartAuthentication(request, response, chain,  
             new InsufficientAuthenticationException()  
    }  
    else {  
       
       this.accessDeniedHandler.handle(request, response, exception);  
    }  
}
```

## Spring MVC에서..
Spring MVC를 사용하여 컨트롤러의 파라미터로 Authentication을 반환받을 수 있다. 하지만 익명 Authentication과 관련하여 유의해야할 점이 있다.
```java
@GetMapping("/")
public String method(Authentication authentication) {
	if (authentication instanceof AnonymousAuthenticationToken) {
		return "anonymous";
	} else {
		return "not anonymous";
	}
}
```

Authentication을 처리하는 ParameterArgumentResolver는 만약 익명 Authentication 상태라면 Authentication에 null을 반환한다. 따라서 위 코드에서 익명으로 접근하더라도 anonymous가 출력될 일은 없다.

만약 컨트롤러에서 이를 확인하고 싶다면 @CurrentSecurityContext를 사용해야 한다.
```java
@GetMapping("/")
public String method(@CurrentSecurityContext SecurityContext context) {
	return context.getAuthentication().getName();
}
```