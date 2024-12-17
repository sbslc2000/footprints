---
상위 링크: "[[Behavioral Pattern]]"
---
# Mediator Pattern
## Pupose
> Allows loose coupling by encapsulating the way disparate sets of objects interact and communicate with each other

서로 다른 객체 집합 간의 상호작용과 통신 방식을 캡슐화하여 느슨한 결합을 가능하게 한다.
## Use When
- 객체 집합 간의 **통신이 명확하면서도 복잡**할 때
- 너무 많은 관계가 존재하여 **공통된 제어 지점이나 통신 지점***common point of control or communication*이 필요한 경우

## Battling Class Complexity
- Mediator 패턴은 일종의 관제탑과 같다.
	- 공항 근처에는 많은 비행기들이 있다.
    - 이들이 하늘에서 서로 소통하게 된다면, 상공은 혼돈 그 자체일 것이다.
    - 허용가능한 해결법 중 하나는 관제탑이다 : 비행기들은 관제탑과 통신하여 착륙에 대한 허가를 구한다.
    - 비행기들은 서로에 대해 알지 못한다.
    - 통신 정보는 관제탑에 유지된다.
    - N:N의 소통관계를 1:N으로 단순화
![](https://i.imgur.com/HmAsAdd.png)
- 위와 같은 사고과정을 프로그램의 객체와 클래스로 추상화해보자.
    - 객체들이 서로 직접 소통하게 만든다면(N:M), 이들 간에는 강한 결합관계가 생긴다.
    - 객체들이 다른 이들에게 메시지를 전송하고 싶을 때, 마치 관제탑처럼 이들의 요청을 받아 전달해줄 수 있는 참여자가 필요하다.
    - 이러한 관제탑 객체는 통신 및 정보 분배의 책임을 갖는다. _Keep the dispatching information inside the new controller_
    - 이러한 협력 객체를 Mediator 라고 부른다.
![](https://i.imgur.com/wGtJvKG.png)

## Class Diagram
![](https://i.imgur.com/HJPscd9.png)

## Object Diagram
![](https://i.imgur.com/FM9rn6C.png)

## Design Principle and Pros & Cons
- 객체들의 상호 연결을 Mediator를 통해 캡슐화한다.
    - 이는 소통 허브_communications hub_의 역할을 한다.
    - 객체_colleagues_들의 상호작용을 협력_coordinating_하고 조율_controlling_하는 책임
- 이는 클래스 간 약한 결합을 촉진한다.
    - 이들이 직접 명시적으로 호출하는 것을 막는다.
    - Mediator는 GUI 컴포넌트에서 주로 사용된다.
- (-) 미디에이터 패턴은 오브젝트들의 소통 로직을 알고 있어야 하므로, 서로 다른 프로그램에서 재사용되기 어렵다_application-specific_.
- (+) 메디에이터 클래스만 보고서도 소통 로직을 이해할 수 있다.

## Decoupling Senders and Receivers
- 옵저버 패턴
    - 인터페이스를 통해 서브젝트와 옵저버의 결합을 낮춘다.
    - 하나의 서브젝트는 여러 옵저버를 가질 수 있다.
    - 옵저버의 수는 런타임에 바뀐다.
    - 객체 간 데이터 의존성의 결합도를 낮추는 좋은 방법이다.

![](https://i.imgur.com/ynGujSW.png)

- 메디에이터 패턴
    - 메디에이터 객체를 통해 객체(동료)들의 결합도를 낮춘다.
    - 동료 객체들의 요청을 라우팅한다.
    - 동료 객체들 간의 통신을 중앙화_centralize_한다.

![](https://i.imgur.com/qb0W9fi.png)

## Communication: Encapsulate vs. Distribute
- 메디에이터
    - 객체 간 소통을 캡슐화한다.
    - 중앙관리적인 소통 방식
    - 메디에이터에서 소통 제약을 유지한다.
    - (-) 메디에이터는 재사용되기 어렵다.
    - (+) 객체간 소통 흐름을 이해하기 쉽다.
- 옵저버
    - 옵저버와 서브젝트 객체를 도입함으로써 소통을 분산시킨다.
    - 제약을 유지하기 위해 옵저버와 서브젝트는 협업한다.
    - (+) 옵저버와 서브젝트는 재사용되기에 유리하다.
    - (-) 소통 흐름을 이해하기 어렵다.