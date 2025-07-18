---
상위 개념: "[[Network Layer]]"
---
# SDN
소프트웨어 정의 네트워킹(SDN)이란 네트워크 기능을 소프트웨어로 정의하는 새로운 접근 방식이다. 이는 네트워크와 인프라 관리 및 제어를 혁신하기 위한 핵심 개념이다.

SDN의 주요 특징은 다음과 같다.

* 네트워크의 데이터 평면과 제어 평면의 분리 : 패킷 전달과 라우팅 결정 기능이 명확히 분리된다.
* 논리적으로 중앙 집중화된 제어 : 제어 기능은 일반적으로 원격 서버에서 관리되는 중앙 집중화식 SDN 컨트롤러라는 별도의 서비스로 구현된다.
* '매치 앤 액션' 추상화 : SDN에서는 패킷 스위치에 들어오는 패킷의 다양한 헤더 필드를 검사하여 일치할 경우 미리 정의된 동작을 수행한다. 이는 단순히 목적지 기반으로 포워딩하는 전통적인 방식보다 훨씬 유연하다.