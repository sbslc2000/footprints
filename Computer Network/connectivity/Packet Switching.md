---
상위 링크: "[[Indirect Connectivity]]"
---
# Packet Switching
Packet Switching은 [[Circuit Switching]]과 다르게, 사전에 링크를 수립하지 않는다. 대신 각각의 데이터 전송 단위를 패킷으로 나누고, 해당 패킷을 보내기 위해 각 라우터에서 스위칭 작업을 동적으로 수행한다.

따라서 각각의 라우터는 store and forward 작업을 수행해야한다. 
1. store : 패킷을 전달받으면 버퍼 공간에 저장해놓는다.
2. forward: 이후 패킷을 분석하여 도착지를 판단한 뒤 해당 링크로 패킷을 전송한다.

이 방법은 Circuit Switching의 dedicate link문제를 해결하고, 컴퓨터 통신의 트래픽에 걸맞게 사용자가 원할 때 원하는 만큼만 데이터를 보낼 수 있는 기반이 된다.

## 단점
한 편, 한 번 회선만 수립하면 간섭 없이 비트를 송수신할 수 있는 Circuit Switching과는 다르게, Packet Switching에서 데이터는 매번 노드에 저장된 후, 해석되고, 다시 전송되는데에 추가적인 시간 및 공간 비용이 필요하여 지연이 있을 수 밖에 없다. 

또한 회선 스위칭에서는 데이터만 보내면 이미 완성된 링크를 통해 데이터가 흘러갔지만, 패킷 스위칭에서 데이터를 보내기 위해서는 어디로 갈지에 대한 정보를 함께 부착하여 전달해야한다. 이러한 정보를 헤더라고 한다.