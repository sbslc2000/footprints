# Packet Scheduling
큐에 있는 패킷이 출력 링크를 통해 전송되는 순서를 결정하는 작업을 패킷 스케쥴링(Packet Scheduling)이라고 한다.

## FIFO
![](https://i.imgur.com/BsDJSSR.png)
![](https://i.imgur.com/QF9BY6B.png)
FIFO는 출력 링크 큐에 도착한 순서와 동일한 순서로 출력 링크에서 전송할 패킷을 선택하는 정책이다. FCFS(First Come, First Served)라고도 불린다.

## Priority Queueing
![](https://i.imgur.com/XUkhpmL.png)
![](https://i.imgur.com/RizesX8.png)
우선순위 큐잉(Priority Queueing)에서 출력 링크에 도착한 패킷은 우선순위 클래스로 분류된다. 각각의 우선순위에 대해 각각의 큐가 존재하며, 전송할 패킷을 선택할 때 우선순위 큐는 전송 대기 중인 패킷으로 차 있는 상태이면서 가장 높은 우선순위 클래스에서 패킷을 전송한다. 우선순위가 동일한 패킷들 중에서의 선택은 일반적으로 FIFO로 행해진다.

우선순위 큐잉을 통해 네트워크 관리 정보를 우선하는 패킷이나 실시간 VoIP와 같은 실시간성이 중요한 트래픽에 대해 우선적으로 전송하게 만들 수 있다. 하지만 우선순위가 낮은 패킷이 지속적으로 전송의 기회를 보장받지 못하는 기아 상태(starvation)가 발생할 수 있다.

## Round Robin과 WFQ

![](https://i.imgur.com/tld7JKk.png)
라운드 로빈(Round-Robin)에서 패킷들은 클래스로 분류되지만, 각각의 클래스 번갈아가며 패킷을 전송하게 된다.
![](https://i.imgur.com/G7wo0DK.png)
라우터에서 널리 구현된 라운드 로빈 큐잉의 일반화된 형태는 **WFQ**(Weighted Fair Queueing)이다. WFQ는 각 클래스마다 다른 양의 서비스 시간을 부여받는 점에서 라운드 로빈과 차이가 있다.각 클래스는 가중치가 있어, 각 클래스별로 대역폭에서의 가중치만큼의 시간을 보장받는다.
