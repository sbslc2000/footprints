---
상위 개념: "[[../Security|Security]]"
---
# Cross-Site Scripting(XSS)
크로스 사이트 스크립팅(XSS, Cross-Site Scripting) 취약점은 인터넷 전반에서 가장 흔하게 발견되는 취약점 중 하나로, 웹 애플리케이션이 사용자의 브라우저에서 스크립트를 실행한다는 점을 악용한다.

XSS 공격은 여러 방식으로 분류되며, 그 중 대표적인 세 가지는 다음과 같다.

* Stored XSS: 악의적인 스크립트 코드가 데이터베이스에 저장되어 이후 사용자들의 조회를 통해 노출되고 실행되는 케이스
* Reflected XSS: 악의적인 스크립트 코드가 데이터베이스에 저장되지는 않지만, 서버에 의해 반사되는 경우
* DOM-based XSS: 악의적인 스크립트가 브라우저 안에서 저장되고 실행되는 경우

오픈 웹 애플리케이션 보안 프로젝트(OWASP, Open Web Application Security Projecct) 같은 위원회도 이 세 가지 XSS 공격 유형을 웹에서 가장 흔한 공격 벡터로 지정해 왔다.