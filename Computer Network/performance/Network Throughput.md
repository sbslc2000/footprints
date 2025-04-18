---
상위 개념: "[[Network Performance]]"
---
# Network Throughput

![](https://i.imgur.com/ci6A2Tw.png)

위 a 예시에 각 링크의 전송률 $R_s$ 와 $R_c$ 중 네트워크의 처리율은 가장 작은 전송률을 갖는 링크에 종속된다. 이 때 가장 작은 링크를 병목 링크*bottleneck link*라고 하며, 병목 링크의 전송률이 곧 처리율이 된다. 

![](https://i.imgur.com/czWyNJ9.png)
처리율이 항상 경로 링크의 전송률에 의존하지는 않는다. 경로 링크를 여러 사용자들이 사용하여 간섭 트래픽이 발생하는 경우에도 처리율은 떨어질 수 있다. 위 예시는 인터넷 코어가 충분한 전송률을 제공하지 못하여 병목이 되는 경우의 예이다. 하지만 오늘날의 인터넷 코어는 매우 높은 속도의 링크로 구성되어 있어, 대부분의 병목 지점은 접속 네트워크(각 호스트와 연결된 라우터 간의 링크)에서 발생한다.