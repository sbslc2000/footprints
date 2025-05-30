---
공식 링크: https://mqtt.org/
상위 개념: "[[../Application Layer/Application Layer|Application Layer]]"
---
# MQTT
MQTT란 machine-to-machine 통신에 사용되는 메시징 프로토콜이다. 이는 경량으로 설계되었는데,  IoT와 같이 리소스 제약이 있는 상황에서의 데이터 전수송을 목적으로 설계되었기 때문이다. MQTT의 컨트롤 메시지는 2 byte 정도이며, 작업 시 아주 적은 전력만을 필요로 한다.

## 구성요소
MQTT 프로토콜은 Publish/Subscribe 모델을 기반으로 작동한다.

### MQTT Client
MQTT 클라이언트는 MQTT 라이브러리를 실행하는 모든 디바이스가 될 수 있다. 클라이언트는 메시지를 게시할 수도, 수신할 수도 있다.

### MQTT 브로커
MQTT 브로커는 여러 클라이언트 간의 메시지를 조정하는 백엔드 시스템이다. 메시지를 수신 및 필터링하고, 각 메시지를 구독하는 클라이언트를 식별하며 메시지를 전송하는 작업을 담당한다.

## 동작 방식
1. MQTT 클라이언트의 MQTT 브로커와의 연결
이는 클라이언트가 CONNECT 메시지를 브로커에게 보내고, 브로커가 CONNACK 메시지로 응답함으로써 확인된다.
2. 클라이언트에서는 메시지를 게시하거나 구독
3. MQTT 브로커는 메시지를 수신한 후 구독자에게 메시지를 전달

### MQTT Topic
MQTT에서 Topic은 폴더 디렉터리와 유사한 계층 구조로 정렬된다.
Ex. ourhome/groundfloor/livingroom/light

