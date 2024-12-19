---
상위 개념: "[[Process]]"
---
Interprocess Communication은 프로세스들이 데이터를 보내고나 받을 수 있는 기법들을 의미한다.

# 클라이언트 서버 환경에서의 통신

## Socket
[[Socket]]은 통신의 극점을 뜻하며, 서로 다른 네트워크의 두 프로세스가 통신할 때 사용한다.

## RPC
Remote Procedure Calls는 네트워크로 연결된 두 시스템이 통신하는 방법을 프로시저 호출 기법으로 추상화 한 것이다. RPC 통신에서 메시지는 원격 포트의 RPC 데몬의 주소와 실행할 함수의 식별자, 매개변수로 구성된다.

**Stub** : 클라이언트와 서버 사이의 인터페이스를 추상화한 소프트웨어 모듈로, RPC 원격 호출을 처리하는 코드로 제공된다

**Parameter Marshalling (파라미터 정리)** : 클라이언트와 서버 기기의 데이터 표현 방식의 차이 문제를 해결하는 것으로, 일반적으로 XDR 이라는 중립적인 형태로 바꾸어 전송하고, 받는 측에서도 다시 서버의 데이터 표현형태로 바꾸어 해석한다.