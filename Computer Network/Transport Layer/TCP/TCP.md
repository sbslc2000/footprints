---
상위 개념: "[[Transport Layer]]"
---
# TCP
TCP(Transmission Control Protocol)는 트랜스포트 계층의 프로토콜로, 연결지향-신뢰적인 데이터 송수신 서비스를 제공한다. TCP는 RFC 793, RFC 1122, RFC 2018, RFC 5681, RFC 7323에 정의되어 있다.

## 주요 특징
* 연결지향형(connection-oriented)
* 전이중 서비스(full-duplex service)
* 신뢰적인 데이터 전송
* 혼잡도 관리(congestion control)

## 세그먼트 구조
TCP는 다양한 기능을 제공하는 만큼 헤더의 구성이 복잡하다.
![](https://i.imgur.com/CNqvr5g.png)
* Source Port #(16), Dest Port #(16) : 다중화와 역다중화를 위해 사용한다.
* Checksum(16): 오류 검증을 위해 사용한다.
* Sequence Number(32), Acknowledgement Number(32): 순서번호와 ACK번호에 대한 필드이다.이들은 모든 패킷에 존재하여, 하나의 패킷은 ACK과 데이터를 동시에 전송할 수 있다.
* Receive Window(16): 수신자가 받아들이려는 바이트의 크기를 나타내며, 흐름 제어(flow control)의 목적으로 사용된다.
* Header Length(4): 32비트 워드 단위로 TCP 헤더의 길이를 나타낸다. TCP의 헤더는 options 필드가 존재하여 가변 길이를 갖기 때문이다.
* Option Field: 송신자와 수신자가 최대 세그먼트 크기(MSS)를 협상하거나 고속 네트워크에서 사용하기 위한 윈도 확장 요소로 이용된다.
* 각종 플래그 필드 (6)
	* ACK: ACK 비트의 유효성을 의미
	* RST, SYN, FIN: 연결 설정과 해제에 사용
	* PSH: 이 데이터를 상위 계층에 즉시 전달하라는 의미로 사용
	* URG: 송신 측 상위 계층 개체가 '긴급'으로 표시하였음을 의미