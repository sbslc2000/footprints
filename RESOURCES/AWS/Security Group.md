> 보안 그룹은 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 합니다.

# Concept 
* Network Access Control List 와 함께 방화벽의 역할을 하는 서비스
* 트래픽 허용 Port 를 설정하는 방식으로 구동됨
	* 기본적으로 모든 포트는 비활성화
	* 선택적으로 트래픽이 지나갈 수 있는 포트와 source를 설정 가능
	* **Deny는 불가능** -> deny 는 NACL로 가능함
* 인스턴스 단위(정확히는 ENI 단위)
	* 하나의 인스턴스에 하나 이상의 SG 설정 가능
	* NACL의 경우 서브넷 단위
	* 설정된 인스턴스는 설정한 모든 SG의 룰을 적용받음
![](https://i.imgur.com/nUc82BC.png)


## Stateful
보안그룹에서 Stateful은 Inbound로 들어온 트래픽이 별 다른 Outbound 설정 없이 나갈 수 있다는 뜻이다.
![](https://i.imgur.com/vCBndol.png)

이때 SG의 outbound 트래픽이 모두 비허용 되어있어도, 클라이언트는 응답을 받을 수 있다. 반면 NACL의 경우 Stateless 방식을 채택하여 OutBound에 허용된 포트로만 트래픽을 보낼 수 있다. NACL의 경우 임의포트의 range만을 outbound traffic으로 전송할 수 있도록 allow해주어야 한다.

## NACL과는 무엇이 다른가?
NACL은 보안그룹과 같이 방화벽 역할을 하지만, 서브넷 단위로 사용되며 포트 및 IP를 직접 Deny할 수 있다.
![](https://i.imgur.com/gS7QomJ.png)

## Source의 종류
트래픽을 허용하는 기준을 다음과 같이 설정할 수 있다.

* IP Range(CIDR)
* Prefix List
* 다른 보안그룹

### Prefix List
Prefix List는 하나 이상의 CIDR 블록의 집합이며, 보안 그룹 혹은 Route Table에서 많은 대상을 참조하기 위해 사용한다. IPv4, IPv6 둘 다 사용 가능하지만, 하나의 Prefix List에 두 가지 타입을 동시에 사용하는 것은 불가능하다. 
* 고객 관리형 Prefix List: 직접 IP주소를 생성/수정/삭제할 수 있으며 다른 계정과도 공유 가능
* AWS 관리형 Prefix List: AWS 서비스를 위한 IP목록으로 조회만 가능
![](https://i.imgur.com/BrYjzSF.png)
![](https://i.imgur.com/a79e4cI.png)

위 경우 서버를 구성하는 PC가 늘어나고 줄어들 때마다 보안 그룹에 해당하는 소스를 동시에 추가하고 삭제해주어야 하여 관리에 어려움이 있다.

![](https://i.imgur.com/ACXupTz.png)

반면 위와 같이 접두사 목록을 사용하면 변경에 덜 취약해진다. -> 테이블을 참조하는 방식

### 보안 그룹 참조 
![](https://i.imgur.com/UY3myJU.png)

Auto Scaling의 경우 트래픽의 양에 따라 구성하는 가상 서버 대수의 크기가 변화함 뿐만 아니라 IP도 변화한다.

이 경우 IP Range를 통해 소스를 설정하는 것은 좋지 못하다. 예를 들어 위에서 10.1.1.1 부터 10.1.1.24 까지 모두 적용시키려는 경우, 서브넷 내의 다른 인스턴스들에게 간섭받을 가능성이 생긴다. 다양한 서브넷의 인스턴스들이 RDS에 접근하려는 경우도 효율적인 해결방안이 되지 않는다. 따라서 이 경우는 보안그룹 참조를 사용해야한다.
![](https://i.imgur.com/GUpS1qj.png)
이 경우 RDS는 Seg-EC2 보안그룹에 속해있는 모든 인스턴스에게 포트를 open한다.

# 보안그룹의 한계점

* Region ekd 2,500개만 생성할 수 있다.
* Inbound, Outbound 규칙은 60개로 제한된다.
* ENI당 최대 보안 그룹은 5개이고, 최대 16개까지 증가 가능하다.
* 한 인스턴스당 최대 적용 가능 규칙은 1000개이다.

