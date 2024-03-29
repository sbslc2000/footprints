# 이벤트 기반 프로그래밍
이벤트 기반 프로그래밍, 이벤트 기반 아키텍처는 애플리케이션 설계를 위한 소프트웨어 아키텍처 모델로, 분리된 서비스 간의 이벤트를 캡처하고 전달하고 처리하는 방식으로 동작한다.

이벤트 기반 프로그래밍은 특히 MSA 서비스에서 분리된 서비스간의 의존성을 최소화하는데에 효과적인 방법이다.

## Terminologies
* Event : 데이터의 변경, 생성, 삭제를 통해 발생하는 서비스의 의미있는 변화
* **Event Driven MicroService** : MSA가 적용된 시스템에서 이벤트 발생시 해당 이벤트 로그를 보관하고 이를 기반으로 동작하며, 비동기 통신을 통해 시스템 내 통합을 수행하는 아키텍처

## 이벤트 기반 아키텍처 모델

### subscribe-publish model
이벤트 스트림 구독을 기반으로 하는 메시징 인프라스트럭처를 의미한다. 이 모델을 사용한다면 이벤트가 발생한 이후 이에 대한 알림을 받아야하는 구독자들에게 이벤트가 전송된다.
### event streaming model
이벤트 사용자는 이벤트를 구독하지 않으며, 이벤트 스트림의 특정 위치를 읽고 처리한다.

![](https://i.imgur.com/WsQO7Rq.png)
