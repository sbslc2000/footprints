---
상위 링크: "[[IP]]"
---
# IP Header
IP 헤더는 총 20 바이트로 구성되어있다. 
![](https://i.imgur.com/OxXoI32.png)

## Version
4 bit가 할당되어있으며, 현재 사용되는 값은 4버전과 6버전이다. 수신측은 이 값에 입각하여 패킷 해석 방식을 바꾼다.

## Header Length 
IP의 헤더는 가변 길이이다. 따라서 4 bit로 헤더 길이를 표현하여 데이터그램에서 페이로드가 시작하는 곳을 결정한다. 대부분의 IP 데이터그램은 옵션을 포함하지 않으므로, 일반적인 경우 헤더의 길이는 20 byte이다. (0101)

## Type of Service
IP가 나르는 데이터의 서비스를 달리하고자할 목적으로 만들었지만, 사용하지 않는다 (ex. 실시간 데이터그램과 비실시간 트래픽). 최근에는 혼잡 상황에서 어떤 것을 버릴 지 우선순위를 정할 때 사용하자는 제안이 있었지만, 기각되었다. 이 필드를 사용하여 제공될 특정 서비스 레벨은 해당 라우터의 네트워크 관리자가 결정하고 구성할 정책 문제이다.

## Datagram Length
바이트로 계산한 IP 데이터그램의 전체 길이이다. 필드의 길이는 16이므로 이론 상 65,535 바이트까지 지원할 수 있다. 

## Identification
16 bit로, 어떤 패킷으로부터 단편화되었는지 표현하기 위해 사용한다.

## Flag/Offset
Offset은 단편화된 패킷을 조립할 때, Flags는 여러 다양한 단편화 관련 동작 및 의사결정을 위해 사용한다. Offset 값의 단위는 바이트이다.

## TTL
8 bit이 할당되어있으며, 라우터를 거칠 때마다 줄어들어 패킷의 수명을 결정한다. 네트워크에서 데이터그램이 무한히 순환하지 않도록 한다. TTL이 0이되면 라우터는 데이터그램을 폐기한다.

## Protocol
IP 데이터그램이 최종 목적지에 도달했을 때 사용되는 역다중화 키이다. 수신측에서 해석하고 상위 계층 프로토콜에 올려보낼 때 사용한다. 값이 6이라면 TCP로, 값이 17이라면 UDP로 데이터를 전달한다.

가능한 모든 값의 목록은 [IANA Protocol Numbers 2016](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml)에 있다.

## Checksum
헤더에 대한 체크섬이며 비트 오류를 탐지하는데 도움을 준다. 체크섬은 매 라우터를 통과할 때마다 재계산되어야 하는데, 그 이유는 TTL과 옵션 필드의 값이 변경되기 때문이다.

성능 이슈로 실질적으로 사용되지는 않는다.

>[!info] TCP/IP는 왜 트랜스포트 계층과 네트워크 계층에서 각각 오류검사를 수행하나요?
> 1. IP 헤더만 IP 계층에서 체크섬을 수행하고, TCP/UDP 체크섬은 전체 세그먼트를 계산한다.
> 2. TCP/UDP는 동일한 프로토콜 스택에 속할 필요가 없다. 원리상 TCP는 IP가 아닌 다른 네트워크 프로토콜(예: ATM) 위에서 운영될 수 있고, IP는 TCP나 UDP로 전달되지 않는 데이터를 전달할 수도 있다.

## DestAddr & SrcAddr
발신지와 목적지 주소이다. 데이터그램의 출발지와 발신지이다! 라우터를 경유하더라도 변경되지 않는다.

## Option
옵션 필드는 IP의 헤더를 확장한다. 그러나 이 역시 거의 사용되지 않는다.
데이터그램의 헤더가 가변길이가 되면 최적화에 어려움이 생기고, 라우터에서 IP 데이터그램을 처리하기 위해 필요한 시간이 크게 달라지기 때문에 고성능 라우터에 부적합하기 때문이다. 
