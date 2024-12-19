---
상위 링크: "[[Data Link Layer]]"
---
# Ethernet
Ethernet은 로컬 영역 네트워크 ([[LAN]])에서 장치 간 데이터를 효율적이고 신뢰성있게 전송하기 위해 설계된 기술이다. 하나의 링크를 여러 호스트가 나누어 사용할 때에 공평하게 접근할 수 있는 알고리즘을 제공한다.
## Bus Topology
![](https://i.imgur.com/QR265ri.png)

고전적 이더넷은 하나의 회선을 여러 호스트가 공유하는 LAN 환경을 지원하기 위해 설계되었다. 아래 CSMA/CD가 필요한 이유를 알기 위해서는 동축 케이블의 특징을 알아야 한다.

동축 케이블은 하나의 물리적 매체로 여러 장치를 직렬로 연결하는 버스형 토폴로지에서 사용된다. [모든 장치는 하나의 케이블을 통해 데이터를 전송하고 수신한다](Multiple%20Access).

따라서 호스트 A와 호스트 B가 매체에 동시에 신호를 전송할 수는 없다. 신호는 **충돌***collision*한다. 따라서 A가 신호를 전송할 때에는 B는 기다리는 제어 규칙을 만들어주어야 한다. 공평한 제어 규칙을 만드는 것은 이더넷의 과제이기도 했다.

버스 토폴로지의 두번째 특징은 모든 데이터 송신은 브로드캐스팅 된다는 것이다. A가 B에게 신호를 보낼 때, 옆에 있던 C 역시 신호를 받는다. 따라서 각 랜카드는 신호를 받고 나서 호스트로 보낼지 아니면 폐기할지를 결정해주어야 한다.

## CSMA/CD
### 반송파 감지 다중 접속 Carrier Sense Multiple Access
이더넷 랜카드는 데이터를 송신하기 전 회선에 신호가 흘러가고 있는지 미리 검사한다. 만약 회선이 유휴 상태라면 데이터를 즉시 전송하고, 회선이 다른 호스트에 의해 사용중이라면 유휴 상태가 될 때까지 기다렸다가 즉시 전송한다.

### 충돌 감지 Collision Detect
CS를 통해 유휴 상태의 회선에 데이터를 전송하기로 결정하였더라도, 회선의 상태를 점검한다. 서로 다른 호스트에서 CS를 거의 동일한 시간에 수행하고 데이터를 전송하겠다는 결정을 한다면 충돌이 일어날 수 있기 때문이다.

충돌이 발생했다는 것은 전압을 체크하는 것으로 쉽게 감지할 수 있다. 만약 전압에 이상이 생긴 경우, 충돌이 발생한 것으로 간주하고 충돌이 발생했다는 의미의 '잼' 신호를 보낸 뒤 프레임 전송을 멈춘다.

그런데, 충돌 감지는 전송하는 모든 순간에 해야하나? 사실은 그렇지는 않다. 모든 회선을 전송하는 신호가 덮는다면,  그 이후에는 다른 호스트들이 CS에 의하여 전송을 하지 않을 것이므로, 더 이상 충돌이 발생하지 않고 따라서 감지할 필요가 없다. 결국 회선을 덮을 수 있을 정도의 시간 동안은 충돌 감지를 해야한다.

IEEE 802.3 표준에서 동축 케이블 기반 이더넷은 최대 5개의 세그먼트와 4개의 중계기, 그리고 동축 케이블의 총 길이는 2500m를 넘지 않게 해야한다고 결정했다. 이는 신호 감쇠와 전파지연시간 등을 고려해서 결정한 내용들이다. 결국 신호가 회선을 다 덮었는지에 확신할 수 있는 시간을 따지려면 위 규칙 내에서 최악의 상황을 고려해야한다.

![430ZVFq.png](https://i.imgur.com/430ZVFq.png)

이더넷에서 노드 A와 노드 B가 네트워크의 양 끝에 위치한 상황을 가정해보자. 노드 A가 데이터를 송신하고 노드 B가 이를 수신하기까지는 시간이 걸리며, 그동안 노드 B는 A가 데이터를 송신 중이라는 사실을 알 수 없다. 이로 인해 노드 B가 데이터를 송신할 경우, 네트워크 상에서 충돌이 발생할 가능성이 높아진다. 특히, A가 송신한 프레임의 크기가 매우 작다면 문제는 더욱 심각해진다. A의 프레임이 네트워크를 따라 전파되다가, B의 프레임과 충돌한 이후에 위치한 다른 노드들에게는 손상된 상태로 전달될 수 있다.

이 문제를 해결하기 위해 이더넷은 네트워크의 가장 먼 노드까지 신호가 전파되었다가 돌아오는 시간(왕복 전파 시간, RTT) 동안 충돌을 감지할 수 있어야 한다는 원칙을 설정하였다. 이를 보장하기 위해 **최소 프레임 크기**가 정의되었다. 최소 프레임 크기는 송신 노드가 신호를 전송하는 동안 충돌이 발생하더라도 이를 감지하고 적절히 대응할 수 있는 시간을 확보하기 위해 설정된 것이다. 이 기준에 따라 이더넷의 최소 프레임 크기는 64바이트(헤더 포함)로 규정되어 있으며, 이는 충돌 감지가 제대로 이루어질 수 있도록 충분한 송신 시간을 보장한다.