---
상위 개념: "[[HTTP]]"
---
# Cookie
HTTP 서버는 무상태성을 갖는다. 이는 서버의 설계를 간단하게 만들고, 동시에 여러개의 TCP 연결을 다룰 수 있는 고성능의 웹 서버를 만들 수 있게 해주는 기반이 된다. 하지만, 서버가 사용자의 접속을 제한하거나 사용자에 따른 콘텐츠를 제공하는 것이 바람직할 때가 존재한다. 이 목적으로 HTTP는 Cookie \[RFC 6265]를 사용한다.

## 동작 원리
![](https://i.imgur.com/ak3Tes3.png)

사용자는 Amazon 웹 서버에 처음으로 접근한다. 이후 Amazon 웹서버는 해당 HTTP 요청에 Cookie 값이 없음을 확인하고, 새로운 사용자로 인식한다. 해당 사용자의 ID (위 예제에서 1678)를 만들어 엔트리에 저장한 뒤, HTTP 응답에 `Set-cookie: 1678`을 담아 전송한다.

브라우저가 HTTP 응답을 받았을 때 `Set-cookie` 헤더가 존재한다면 브라우저가 관리하는 파일에 이를 저장한다. 사용자가 다음번 Amazon 웹서버에 접속할 때에는 파일에서 이를 가져와 `cookie: 1678`이라는 헤더를 HTTP 요청에 담아 전송한다.

Amazon 웹 서버는 Cookie 값을 확인하여, 기존에 접속 이력이 있는 사용자임을 인식한다. 이후 해당 식별자를 통해 서버의 정보와 조합하여 컨텐츠를 제공한다.

## Properties

### Domain
```text
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
```
도메인 속성은 쿠키를 수신할 수 있는 호스트를 지정한다. 즉 쿠키를 저장한 이후, 요청을 보낼 때 해당 호스트의 도메인이 쿠키의 도메인과 일치하는 경우에만 쿠키를 전송한다.

이는 하위 도메인 역시 포함된다. 예를 들어 Domain=example.com 인 쿠키는 admin.example.com 에 요청을 보낼 때에도 전송된다.

만약 Domain을 생략한다면, 기본적으로 해당 쿠키를 발급한 URL의 호스트로 Domain 값이 설정된다.

### Path
```
Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
```
Path는 쿠키의 유효한 경로 범위를 제한하는 역할을 한다. 예를 들어 Path=/admin 이 설정되어있다면, /hello로는 쿠키가 전송되지 않고, /admin 혹은 그 하위 경로에 요청할 때에는 쿠키가 전송된다.

### Expires, Max-Age
```code
Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<number>
```

Max_Age는 쿠키가 만료될 때까지의 시간(초)를 나타낸다. 0 또는 음수인 경우 쿠키는 즉시 만료된다. Expires와 Max_Age가 모두 설정되어있다면 Max_Age가 우선된다.

Expires는 쿠키의 최대 수명을 HTTP-date 타임스탬프로 표시한다. 

### HttpOnly
XSS(Cross Site Script) 공격을 방지하기 위해, 자바스크립트가 Document.cookie 속성을 통해 쿠키에 액세스하는 것을 금지한다.

### Secure
https 스키마로 요청할 때에만 쿠키가 전송되는 것을 가능하게 하는 설정이다. localhost는 제외된다.

### SameSite
Cross-site 요청에 대해 쿠키를 보낼 것인지 말 것인지를 결정하는 속성이다. 

site는 프로토콜과 도메인 네임의 마지막 부분만을 포함하는 개념이다. 예를 들어 https://app.example.com:8080 이라는 주소가 있다면, https와 example.com이 같다면 모두 같은 site로 취급된다. 

* Strict : 정확히 site가 동일한 경우에만 쿠키를 서버로 전송한다.
* Lax: 이미지 또는 프레임 로드와 같은 Cross-site 요청에는 쿠키가 전송되지 않지만, 사용자가 외부 사이트에서 origin 사이트로 이동할 때는 쿠키가 전송된다.
* None: Cross-site 및 Same-site 요청 시 모두 쿠키를 전송한다.