---
상위 링크: "[[Network Performance]]"
---
# Latency

Latency란 한 노드에서 다른 노드로 메시지를 송신하는데에 걸리는 시간을 의미한다. 

## Latency에 영향을 미치는 요소

![](https://i.imgur.com/jTGX8EN.png)

Latency는 Propagation, Transmit과 Queue의 합이다.

### Propagation
Propagation(전파지연시간) 이란 다른 노드까지의 거리에 의해 데이터를 전송하는데 걸리는 물리적 시간을 의미한다. 전파의 속도가 일정하다는 가정하에는 거리가 멀수록 전파지연시간은 증가한다.
### Transmit
Transmit(전송시간) 이란 송신자 입장에서 첫 비트를 출발시킨 이후 마지막 비트를 출발시킬 때까지 걸리는 시간이다. 이 시간은 패킷의 크기에 비례하며, 대역폭의 크기에 반비례한다.
### Queue
Queue(큐잉 시간)은 라우팅을 위해 큐에서 대기하는 시간이다.

