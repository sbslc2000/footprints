# Remember me
Remember-me 기능은 웹 세션에서 벗어나더라도 기존에 인증했던 정보를 유지하는 방법이다.
```java
  @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http.rememberMe(Customizer.withDefaults());  
        
        return http.build();  
    }
```
filterChain 설정에서 rememberMe를 활성화 시키면 로그인 창에서 Remember me 체크박스가 생성된다.
![](https://i.imgur.com/fOmc7tB.png)

이후 인증을 수행하면 다음과 같이 session id 뿐만 아니라 remember-me라는 쿠키도 생성되어있는 것을 확인할 수 있다. 이 정보를 활용하여 이제는 JSESSIONID가 유효하지 않아도 인증을 받을 수 있다. remember-me 토큰의 기본 유효 시간은 14일이다.
![](https://i.imgur.com/4itgzfh.png)

## Remember-me Token
Remember-me 토큰은 다음과 같은 내용을 포함한다.
```java
base64(username + ":" + expirationTime + ":" + algorithmName + ":"
algorithmHex(username + ":" + expirationTime + ":" password + ":" + key))
```
Remember-me 토큰은 username과 password가 바뀌지 않았다는 가정 하에서 expirationTime 동안 유효하다.
### Remember-me Token은 안전할까?
Remember-me 토큰을 통한 인증은 내부 키를 통해 복호화 값을 얻어낸 뒤 해당 값을 UserDetailsService를 통해 검증하기 때문에, 일단 토큰이 누출되었다는 것만 인지한다면 비밀번호를 바꾸는 방식으로 발행되었던 토큰을 무효화 하는 것이 가능하다.

## Persistent Token Approach
#todo
[Remember-Me Authentication :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/authentication/rememberme.html#remember-me-persistent-token)

## Interfaces and Implementation
Remember-me의 기능은 `AbstractAuthenticationProcessingFilter`에서 호출된다. 

```java
protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain,  
	...
    this.rememberMeServices.loginSuccess(request, response, authResult);  
	...
}
```

RememberMeService는 다음과 같은 메서드들을 가지고 있다. loginFail과 loginSuccess는 각각 로그인 실패 및 성공한 경우 토큰을 부여하거나 삭제하는 역할을 한다. 이들은 AbstractAuthenticationProcessingFilter에서 호출된다.

autoLogin()은 SecurityContext에 인증 객체가 없는 경우 (세션 만료 및 브라우저 다시 열기 등) 토큰의 정보를 읽어 인증을 처리하기 위해 사용된다.
```java
Authentication autoLogin(HttpServletRequest request, HttpServletResponse response);

void loginFail(HttpServletRequest request, HttpServletResponse response);

void loginSuccess(HttpServletRequest request, HttpServletResponse response,
	Authentication successfulAuthentication);
```

```java
@Override  
protected UserDetails processAutoLoginCookie(String[] cookieTokens, HttpServletRequest request,  
       HttpServletResponse response) {  
    // 대충 쿠키의 형식이 맞는지 확인하는 코드

	// 대충 만료 기한이 지나지 않았는지 확인하는 코드

	//대충 UserDetailsService를 통해 쿠키 정보를 가진 사용자를 가져오는 코드

	// 대충 암호화 알고리즘을 통해 시그니처를 검증하는 코드
	
    return userDetails;  
}
```

### TokenBasedRememberMeServices
TokenBasedRememberMeServices는 RememberMeAuthenticationProvider에 의해 처리되는 RememberMeAuthenticationToken을 생성한다. 
TokenBasedRememberMeServices는 RememberAuthenticationProvider와 Key를 공유한다. 아마 암호화-복호화 때문인 듯.
또한 TokenBasedRememberMeServices는 UserDetailsService를 통해 사용자 데이터를 찾고 검증하기 때문에 DB 호출을 발생시킬 수 있다.

TokenBasedRememberService는 AbstractRememberMeServices를 상속하고 있고, AbstractRememberMeService는 LogoutHandler를 구현한다. 이는 로그아웃시 LogoutFilter에서  Rememberme 토큰을 삭제하기 위함이다.
```java
@Override  
public void logout(HttpServletRequest request, HttpServletResponse response, Authentication authentication) {  
    cancelCookie(request, response);  
}
```
### PersistentTokenBasedRememberMeServices
기본적으로 TokenBasedRememberMeServices와 비슷하게 구현되어 있지만, PersistentTokenRepository를 token 저장 목적으로 사용한다는 점만 다르다.