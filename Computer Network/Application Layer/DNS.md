---
상위 개념: "[[Application Layer]]"
---
# DNS
> DNS 서버들의 계층구조로 구현된 분산 데이터베이스

> 호스트가 분산 데이터베이스로 질의하도록 허락하는 애플리케이션 계층 프로토콜

호스트는 호스트 이름과 IP 주소로 식별할 수 있다. 그 중 사람은 더 외우기 쉬운 호스트 이름을 선호하고, 라우터는 고정 길이와 계층구조를 갖는 IP 주소를 선호한다. **DNS는 이러한 선호 차이를 절충하기 위해 호스트 이름을 IP 주소로 변환해주는 디렉터리 서비스이다.**

## DNS가 제공하는 서비스

### 호스트 앨리어싱(Host Aliasing)
복잡한 호스트 이름을 가진 호스트는 하나 이상의 별명을 가질 수 있다. 예를 들어 전세계적으로 서비스를 제공하는 Facebook은 여러 대의 호스트에서 서비스를 제공한다. 허나 이들은 모두 www.facebook.com 이라는 별명을 갖는다.
```
➜  ~ nslookup www.facebook.com
Server:		168.126.63.1
Address:	168.126.63.1#53

Non-authoritative answer:
www.facebook.com	canonical name = star-mini.c10r.facebook.com.
Name:	star-mini.c10r.facebook.com
Address: 157.240.215.35
```
www.facebook.com 에 대한 IP 주소 질의를 할 때, DNS 서버는 이에 해당하는 정식 호스트 이름(canonical name)의 IP 주소를 제공한다. 정식 호스트 이름은 DNS 서버마다, 지역마다 달라질 수 있다.

### Mail Server Aliasing

### 부하 분산(Load Distribution)
인기 있는 사이트는 여러 종단 시스템에서 수행되며, 각기 다른 IP 주소를 갖는다. DNS 데이터베이스는 이 IP 주소 집합을 가지고 있으며, 사용자의 질의에 집합 전체를 보낸다. 이 과정에서 주소의 순서를 순환식으로 바꾸어 보내고, 클라이언트는 대부분 첫 번째 IP 주소로 HTTP 요청 메시지를 보내므로 트래픽을 분산하는 효과가 난다.


## 동작 원리
DNS는 분산 데이터베이스가 인터넷에서 어떻게 구현될 수 있는지를 보여주는 훌륭한 사례이기도 하다.
![](https://i.imgur.com/ZwYPgf7.png)

DNS 서버는 전세계에 분포되어있으며, 위와 같은 계층을 갖는다.

* Root DNS Servers : 전 세계에 1,000개 이상의 루트 서버 인스턴스가 흩어져 있다. 이들은 TLD 서버의 IP 주소를 제공한다.
* TLD Servers: Top-level Domain 의 약자로, com, org, net, edu, gov 와 같은 상위 레벨 도메인과 kr, uk, fr, ca, jp 와 같은 국가 레벨 도메인에 해당하는 TLD 서버가 있다. TLD 서버는 Authoritative Server들의 주소를 제공한다.
* Authoritative Servers: 마치 www.facebook.com 처럼 인터넷에서 접근하기 쉬운 호스트들의 IP주소를 제공한다. 웹 서비스를 제공하는 기관은 호스트이름을 IP 주소로 매핑하는 공개적인 DNS 레코드를 제공해야한다.

www.facebook.com 의 IP 주소를 얻기 위해서는 제일 먼저 루트 DNS 서버에 접근해야하며, 루트 DNS 서버는 .com 까지의 주소를 확인하고 com DNS 서버의 IP 목록을 제공한다. 이후 com DNS 서버로부터 facebook.com에 해당하는 책임 DNS 서버의 IP를 얻게 되고, 해당 책임 DNS 서버로부터 www.facebook.com 의 IP 주소를 얻을 수 있다.

### Local DNS 서버
![](https://i.imgur.com/WE6kw4t.png)

위 계층 구조에 엄격하게 포함되지는 않지만 ISP 에서 제공하는 로컬 DNS 서버가 존재한다. 로컬 DNS 서버는 DHCP 과정에서 각 클라이언트 컴퓨터에 할당되며, 클라이언트는 IP 주소를 얻기 위해 로컬 DNS 서버에 질의한다.

이 과정에서 DNS 서버는 도메인 이름 - IP 주소 쌍을 캐싱할 수 있다. 예를 들어 dns.nyu.edu에 gaia.cs.umass.edu에 대한 요청이 들어온다면, 그 결과물을 dns.nyu.edu의 로컬에 저장해놓는다. 다른 클라이언트로부터 gaia.cs.umass.edu에 대한 요청이 들어오는 경우, 로컬의 값을 바로 반환한다. 이를 통해 DNS 서버는 호스트 이름에 대한 책임이 없을 때 조차 원하는 IP 주소를 제공할 수 있다.

하지만 호스트 이름과 IP 주소 사이의 매핑은 영구적인 것이 아니기 때문에, DNS 서버는 일반적으로 2일 동안만 정보를 캐싱하고 이후 제거한다.