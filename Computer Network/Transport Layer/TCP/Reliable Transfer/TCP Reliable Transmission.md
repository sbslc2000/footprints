---
상위 개념: "[[../TCP|TCP]]"
---
# TCP Reliable Transmission
인터넷의 네트워크 계층은 비신뢰적이기에 TCP는 신뢰적인 데이터 전송 서비스를 제공해야한다. TCP는 프로세스가 자신의 수신 버퍼로부터 읽은 데이터 스트림이 손상되지 않았으며 손실이나 중복이 없고 순서가 유지된다는 것을 보장한다.

## Timer
각각의 세그먼트에 대해 타이머가 유지될 수도 있겠지만, 이는 상당한 오버헤드를 갖게될 것이다. \[RFC 6298]의 권장되는 TCP 타이머 관리 절차에 따르면 TCP는 오직 단일 재전송 타이머를 이용할 것을 권고하고 있다.

## TCP Data Transmission Trigger
TCP 송신자는 데이터 전송/재전송에 대해 세 가지 주요 이벤트를 갖는다.

1. 상위 애플리케이션으로부터 수신된 데이터

상위 애플리케이션으로부터 데이터를 받는다면 TCP는 세그먼트로 이 데이터를 캡슐화하고, 네트워크 레이어에 세그먼트를 넘긴다. [이 때 데이터 바이트의 첫째 바이트 번호를 필드에 기입한다](TCP%20Sequence%20Number%20and%20Acknowledgement). 이 때 타이머가 다른 세그먼트에 대해 실행중이 아니라면, TCP는 이를 넘길 때 타이머를 시작한다. 이 때 타이머의 시간은 [[TimeoutInterval]] 값으로 설정한다.

2. 타임아웃

타임아웃이 발생하면 타임아웃을 일으킨 세그먼트를 재전송한다. 그리고 TCP는 타이머를 재시작한다.

3. 수신 확인응답 세그먼트 수신

ACK을 받는다면, TCP는 변수 `SendBase`와 ACK의 값을 비교한다. `SendBase`는 ACK을 받지 못한 가장 오래된 바이트의 순서 번호이다. 만약 ACK > SendBase라면, SendBase 변수를 ACK으로 갱신한다. 갱신 이후에도 확인응답이 안된 세그먼트가 존재한다면 타이머를 재시작한다.

## 타임아웃을 두배로
위 이벤트 중 1번과 3번에 대해서는 TimeoutInterval에서 가져오지만, 타임아웃의 경우는 다르다. 만약 타임아웃이 발생하여 세그먼트를 재전송해야할 때, 새로운 타이머는 TimeoutInterval에서 가져오지 않고 기존 타임아웃의 두배의 길이로 설정한다.

이는 제한된 형태의 혼잡 제어를 제공하는 효과를 갖는다. 타임아웃은 주로 라우터 혼잡에 의해 발생하는데, 이 때 출발지에서 지속해서 패킷 재전송을 고집하면 혼잡은 더욱 악화될 것이기 때문이다. TCP는 대신 송신자가 더 긴 간격으로 재전송하도록하여 전송 속도를 줄인다. 이는 이더넷의 [CSMA/CD](Ethernet##CSMA/CD)에서 유사한 개념을 볼 수 있다.