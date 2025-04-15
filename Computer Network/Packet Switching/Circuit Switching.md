---
상위 링크: "[[../Computer Network|Computer Network]]"
---
# Circuit Switching
![](https://i.imgur.com/uMsHJmc.png)

Circuit Switching (회선 스위칭) 은 주로 전화망에서 사용되었던 방법이다. 송신자 A가 수신자 D에게 정보를 보내기 위해서는, 중간에 있는 라우터들이 서로간의 통신을 수립할 수 있게 스위치를 조정해주어야 한다.

일단 회선이 수립되고 나면, 둘 사이에는 연결되어있는 링크가 생기고, 이후 비트스트림을 중간 및 간섭없이 송수신할 수 있다.

이는 마치 점대점 연결이 된 것 같은 효과를 주어 높은 QoS를 유지할 수 있다.

## 단점
Circuit Switching은 상대방과 통신하기 위해 수립 과정을 거쳐야 하여 최초 연결시 시간이 필요하다.

수립과정이 거치고 나면 해당 링크는 두 통신 사용자의 '전용'이 된다. *dedicated circuit* 따라서 해당 링크는 동시간에 공유될 수 없다.

![](https://i.imgur.com/ovTEdCf.png)

컴퓨터 통신에서는 Bustry Traffic의 특징이 있어 통신의 요구가 생겼다가 없어졌다가를 반복하는데, 이 과정에서 '전용'의 문제로 링크가 불필요하게 점유되는 상황이 발생한다.
