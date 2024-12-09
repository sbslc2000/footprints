---
상위 링크: "[[Internetworking Protocol]]"
---
# Subnetting

## 배경 : 기존 IP 주소 구조의 스케일 확장
IP 주소의 본래 의도는 네트워크 부분이 정확히 하나의 물리적 네트워크를 고유하게 식별하도록 하는 것이었습니다. 하지만 이 접근 방식에는 몇 가지 단점이 있습니다. 예를 들어, 내부 네트워크가 많은 대규모 캠퍼스가 인터넷에 연결하려고 한다고 가정해봅시다. 이 경우, 네트워크가 아무리 작더라도 각 네트워크마다 최소한 하나의 클래스 C 네트워크 주소가 필요합니다. 더 심각한 문제는, 255개 이상의 호스트를 가진 네트워크의 경우 클래스 B 주소가 필요하다는 점입니다. 처음에는 이 문제가 그리 크게 느껴지지 않을 수도 있고, 실제로 인터넷이 처음 구상되었을 당시에는 큰 문제가 아니었습니다. 하지만 네트워크 번호는 유한하며, 특히 클래스 B 주소는 클래스 C보다 훨씬 적습니다. 클래스 B 주소는 특히 수요가 높은데, 이는 네트워크가 255개 노드를 초과하여 확장될 가능성을 대비하기 위해 처음부터 클래스 B 주소를 사용하는 것이 더 간편하기 때문입니다. 나중에 클래스 C 네트워크에서 주소가 부족할 경우 모든 호스트의 주소를 다시 할당해야 하는 불편함을 피하려는 것입니다.

여기서 문제가 되는 것은 주소 할당의 비효율성입니다. 예를 들어, 두 개의 노드를 가진 네트워크가 전체 클래스 C 네트워크 주소를 사용하면, 사용 가능한 253개의 주소가 낭비됩니다. 또한, 255개를 약간 초과하는 호스트를 가진 클래스 B 네트워크는 64,000개 이상의 주소를 낭비하게 됩니다.

따라서 물리적 네트워크당 하나의 네트워크 번호를 할당하는 방식은 IP 주소 공간을 우리가 원하는 것보다 훨씬 빠르게 소모시킵니다. 40억 개 이상의 호스트를 연결해야 모든 유효 주소를 소진할 수 있을 것처럼 보이지만, 클래스 B 네트워크의 경우 약 16,000개(2^14)만 연결해도 해당 주소 공간이 소진됩니다. 따라서 네트워크 번호를 더 효율적으로 사용하는 방법을 찾아야 합니다.

여러 네트워크 번호를 할당하는 또 다른 단점은 라우팅을 고려할 때 분명해집니다. 라우팅 프로토콜에 참여하는 노드가 저장해야 하는 상태의 양은 다른 노드의 수에 비례합니다. 또한 인터넷에서의 라우팅은 라우터가 다른 네트워크에 도달하는 방법을 알려주는 **포워딩 테이블**을 구축하는 것으로 이루어집니다. 따라서 사용 중인 네트워크 번호가 많을수록 포워딩 테이블이 커집니다. 큰 포워딩 테이블은 라우터 비용을 증가시키고, 주어진 기술에서 작은 테이블보다 검색 속도가 느릴 수 있어 라우터 성능을 저하시킵니다. 이는 네트워크 번호를 신중히 할당해야 할 또 다른 동기를 제공합니다.

## 개념
서브네팅*Subnetting*은 할당된 네트워크 번호의 총 수를 줄이는 데 첫 단계를 제공합니다. 이 아이디어는 **단일 IP 네트워크 번호를 가져와 그 번호를 여러 물리적 네트워크에 할당하는 것**입니다. 이를 실현하려면 몇 가지 작업이 필요합니다. 첫째, 서브넷은 서로 가까이 있어야 합니다. 이는 인터넷의 먼 지점에서 볼 때, 서브넷들이 **단일 네트워크**처럼 보여야 하기 때문입니다. 즉, 이들 사이에 하나의 네트워크 번호만 보유하고 있어야 합니다. 이 때문에 라우터는 어떤 서브넷으로든 도달하기 위해 하나의 경로만 선택할 수 있으므로, 서브넷들은 동일한 일반적인 방향에 있어야 합니다.

서브네팅을 사용하기에 이상적인 상황은 물리적 네트워크가 많은 대규모 캠퍼스나 기업입니다. 캠퍼스 외부에서는, 캠퍼스 내의 특정 서브넷에 도달하기 위해 알아야 할 정보는 **캠퍼스가 인터넷의 나머지와 연결되는 지점**뿐입니다. 이는 종종 단일 지점에서 이루어지므로, 포워딩 테이블에 하나의 항목만 추가하면 충분합니다. 심지어 캠퍼스가 여러 지점에서 인터넷에 연결되어 있더라도, 캠퍼스 네트워크 내 한 지점에 도달하는 방법을 아는 것은 여전히 좋은 출발점이 됩니다.

하나의 네트워크 번호를 여러 네트워크가 공유할 수 있도록 하는 메커니즘은 **각 서브넷의 모든 노드에 서브넷 마스크를 구성**하는 방식으로 이루어집니다. 간단한 IP 주소 체계에서는 동일한 네트워크에 있는 모든 호스트가 동일한 네트워크 번호를 가져야 합니다. 서브넷 마스크는 서브넷 번호를 도입할 수 있도록 하며, 동일한 물리적 네트워크에 있는 모든 호스트가 동일한 서브넷 번호를 가지게 됩니다. 따라서, **호스트들이 서로 다른 물리적 네트워크에 속하더라도 동일한 네트워크 번호를 공유할 수 있습니다**. 이는 아래 그림에 예시로 나타나 있습니다.

![](https://i.imgur.com/7gPcEXa.png)

서브네팅이 호스트에 의미하는 바는, 이제 호스트가 자신이 연결된 서브넷에 대해 **IP 주소와 서브넷 마스크를 모두 구성**해야 한다는 점입니다. 예를 들어, 아래 그림에서 호스트 H1은 128.96.34.15라는 IP 주소와 255.255.255.128이라는 서브넷 마스크로 구성됩니다. (특정 서브넷에 있는 모든 호스트는 동일한 서브넷 마스크로 구성됩니다. 즉, 서브넷마다 정확히 하나의 서브넷 마스크가 있습니다.)

IP 주소와 서브넷 마스크의 **비트 단위 AND 연산(bitwise AND)** 결과는 해당 호스트 및 같은 서브넷에 속한 모든 호스트의 **서브넷 번호**를 정의합니다. 이 경우, 128.96.34.15와 255.255.255.128의 AND 연산 결과는 128.96.34.0이므로, 이는 그림에서 상단에 위치한 서브넷의 서브넷 번호가 됩니다.

![](https://i.imgur.com/TlhSQ4G.png)

호스트가 특정 IP 주소로 패킷을 보내고자 할 때, 가장 먼저 수행하는 작업은 **자신의 서브넷 마스크**와 **목적지 IP 주소**를 비트 단위 AND 연산(bitwise AND)하는 것입니다. 연산 결과가 **송신 호스트의 서브넷 번호**와 같다면, 해당 목적지 호스트가 같은 서브넷에 있음을 알 수 있으며, 패킷을 서브넷 내에서 직접 전달할 수 있습니다. 하지만 결과가 다르면, 해당 패킷은 라우터로 보내져 다른 서브넷으로 포워딩되어야 합니다. 예를 들어, H1이 H2로 패킷을 보내고자 하는 경우, H1은 자신의 서브넷 마스크(255.255.255.128)와 H2의 주소(128.96.34.139)를 AND 연산하여 128.96.34.128을 얻습니다. 이는 H1의 서브넷 번호(128.96.34.0)와 일치하지 않으므로, H1은 H2가 다른 서브넷에 있음을 알게 됩니다. H1은 서브넷 내에서 직접 H2에 패킷을 전달할 수 없으므로, 기본 라우터 R1에 패킷을 보냅니다.

서브네팅을 도입하면 라우터의 포워딩 테이블도 약간 변경됩니다. 기존의 포워딩 테이블은 (NetworkNum, NextHop) 형식으로 구성되어 있었습니다. 서브네팅을 지원하려면, 테이블은 이제 (SubnetNumber, SubnetMask, NextHop) 형식의 항목을 포함해야 합니다. 테이블에서 올바른 항목을 찾기 위해, 라우터는 각 항목의 서브넷 마스크(SubnetMask)와 패킷의 목적지 주소를 AND 연산합니다. 연산 결과가 해당 항목의 서브넷 번호(SubnetNumber)와 일치하면, 해당 항목이 사용됩니다. 라우터는 이 항목을 사용해 패킷을 지정된 다음 홉 라우터(NextHop)로 전달합니다. 위의 예제 네트워크에서, 라우터 R1은 아래와 같은 항목들을 가질 것입니다.

![](https://i.imgur.com/uRRk1Bd.png)

H1에서 H2로 데이터그램을 전송하는 예를 계속해서 살펴보겠습니다. 라우터 R1은 H2의 주소(128.96.34.139)를 첫 번째 항목의 서브넷 마스크(255.255.255.128)와 AND 연산하여 그 결과(128.96.34.128)를 해당 항목의 네트워크 번호(128.96.34.0)와 비교합니다. 일치하지 않으므로 R1은 다음 항목으로 넘어갑니다. 이번에는 일치가 발생하므로, R1은 데이터그램을 H2로 전달합니다. 이 작업은 H2와 같은 네트워크에 연결된 인터페이스 1을 사용하여 이루어집니다.

이제 데이터그램 포워딩 알고리즘을 다음과 같은 방식으로 설명할 수 있습니다.
```
D = destination IP address  
for each forwarding table entry (SubnetNumber, SubnetMask, NextHop)

D1 = SubnetMask & D if D1 = SubnetNumber

if NextHop is an interface  
deliver datagram directly to destination

else  
deliver datagram to NextHop (a router)
```

비록 이 예시에서는 보여지지 않았지만, 일반적으로 포워딩 테이블에는 기본 경로(default route)가 포함되며, 명시적으로 일치하는 항목이 없을 경우 이를 사용합니다. 이 알고리즘을 단순하게 구현할 경우, 즉 목적지 주소와 서브넷 마스크를 반복적으로 AND 연산하고, 선형적으로 테이블을 검색하는 방식은 매우 비효율적일 수 있습니다.

서브네팅의 중요한 결과 중 하나는 인터넷의 다른 부분들이 세상을 다르게 본다는 점입니다. 가상의 캠퍼스 외부에서는, 라우터들이 하나의 네트워크만을 봅니다. 위의 예에서, **캠퍼스 외부의 라우터들은 그림 8에 있는 네트워크들의 집합을 단지 네트워크 128.96으로 보고, 이를 도달하는 방법을 알려주는 하나의 항목만을 포워딩 테이블에 유지합니다.** 반면, 캠퍼스 내부의 라우터들은 올바른 서브넷으로 패킷을 라우팅할 수 있어야 합니다. 따라서 인터넷의 모든 부분이 정확히 동일한 라우팅 정보를 보는 것은 아닙니다. 이는 라우팅 정보의 집약*aggregation* 예시로, 라우팅 시스템의 확장성에 있어 근본적으로 중요한 요소입니다. 