---
상위 링크: "[[Structural Pattern]]"
---
# Proxy Pattern
## Purpose
> Allows for object level access control by acting a pass through entity or a placeholder object.

객체를 감싼 객체를 만들어 객체 수준의 접근 제어를 제공한다. 프록시 패턴은 다른 객체에 대한 접근을 제어하기 위해 대리인*surrogate*나 자리표시자*placeholder*를 제공한다.

## Use When
* 원본 객체에 대한 접근 제어가 필요할 때
* 객체에 접근할 때 추가적인 기능이 요구될 

## Participants
![](https://i.imgur.com/d9bDVGp.png)


* Subject
	* Proxy와 RealSubject는 모두 Subject 인터페이스를 구현해야한다.
* RealSubject
	* 주로 실제 동작을 수행하는 객체
* Proxy
	* 일종의 대행자
	* Subject에 대한 레퍼런스를 갖고 있어, 필요할 때 요청을 RealSubject로 전달한다.
	* 사용자는 실제로 프록시에 요청을 하게 되지만, 사용자는 이 사실을 모르고 있어야 한다.
	* Subject의 인터페이스를 구현하지만, 그 기능을 구현하지 않고 본인이 수행하는 접근 제어의 기능만 구현한 뒤 필요한 경우 나머지 동작을 RealSubject에 위임한다.

## Class Diagram
![](https://i.imgur.com/ukr3hIP.png)

## Applicability
프록시 패턴은 단순한 포인터보다 더 다재다능하고 정교한 객체 참조가 필요할 때 사용된다.
* **원격 프록시** *remote proxy*
	* 요청과 해당 인수를 인코딩하여, 다른 주소공간에 있는 실제 객체로 요청을 전송
* **가상 프록시** *virtual proxy*
	* 실제 객체에 대한 접근을 지연시킬 수 있도록 추가 정보를 캐싱
* **보호 프록시** *protection proxy*
	* 요청을 수행하기 위해 호출자가 필요한 접근권한을 가지고 있는지 확인