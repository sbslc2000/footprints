---
상위 링크: "[[Amazon Web Service]]"
---
# VPC
Amazon Virtual Privacy Group(VPC)을 사용하면 AWS 클라우드에서 논리적으로 격리된 공간을 프로비저닝하여 고객의 정의하는 가상 네트워크에서 AWS 리소스를 시작할 수 있다.

-> 내가 만든 AWS의 서비스들을 나의 계정이 아닌 계정에서 볼 수 없고, 같은 계정이다 하더라도 다른 지역이라면 볼 수 없다.

또한 같은 VPC 안에 있더라도, 각각의 인스턴스들은 격리되어 있어 통신할 수 없다. 각각의 인스턴스는 트래픽을 송수신할 때  [Security Group](Security%20Group.md)의 정책에 의해 통제된다. 따라서 VPC 내부의 인스턴스간 트래픽을 송수신 시키려면 Security Group의 정책을 바꾸어 주어야 한다.