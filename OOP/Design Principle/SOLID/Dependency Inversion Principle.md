---
상위 개념: "[[SOLID]]"
---
# Dependency Inversion Principle

> High-level modules should not depend on low-level modules. Both should depend on abstractions.


의존성 역전 원칙(Dependency Inversion Principle, DIP)는 객체지향 설계원칙 중 하나이다.

의존성 역전 원칙은 고수준 모듈이 저수준 모듈에 의존성을 가져서는 안된다고 말한다. 

DIP는 구조적 분석설계방법을 따른다면 나타날 수 있는 dependency의 방향을 반대로 뒤집어 준다.

- Why Inversion?
    - DIP attempts to **“invert” the dependencies** that result from a **structured analysis and design approach**

## Typical in Structured Analysis & Design

구조적 분석설계는 Top-down 설계이다. 가장 상위의 Program이라는 모듈이 있다면, 이것의 기능 분해 Function Decomposition을 통해 여러 모듈로 나뉘어지고, 이 친구들이 각각의 기능을 Function이란 단위로 호출하게 된다.

→ 상위 모듈이 하위 모듈에 dependency를 갖는 구조

→ 자주 변경되는 단위의 function이 변경되면, 이 변경의 여파가 위로 쭉쭉 올라간다.
![](https://i.imgur.com/6KFkSzG.png)

## Dependency Inversion Principle

DIP는 모듈간에 인터페이스를 놓아, 상위 모듈과 하위 모듈이 Abstraction layer를 통해 연결되도록 한다. 이 경우 하위 모듈의 의존방향이 역전이 된다.

이때 Class 1의 변경은 그 여파가 전달되지 않는다.

Interfaces and abstract classes are high-level resources Concrete classes are low-level resources

![](https://i.imgur.com/YW1h3Mz.png)

## Inversion of Ownership

DIP 는 Control flow만 역전시키는 것이 아닌 **ownership**도 역전시킨다.

Server 가 Interface 를 implement 하고, Client가 interface를 사용하는 관계에서, DIP는 Client가 interface의 ownership을 갖게 한다. : 사용하는 측의 소유

- Its not just an inversion of **dependency**, DIP also inverts **ownership**
    - Typically a service interface is “owned” or declared by the server, here the client is specifying what they want from the server
    - **DIP → Clients should own the interface!**

Layer 아키텍처에 적용하면 다음과 같다.

이 구조는 구조적 분석설계에서의 레이어 의존 다이어그램이다.

상위 레이어는 하위 레이어를 사용하여 의존관계가 형성된다. 이것은 DIP에 위배되는 상황이다.
![](https://i.imgur.com/tvWz8wv.png)

DIP를 적용한 구조는 다음과 같다. 각각의 레이어는 다른 레이어와 소통할 때 Abstraction layer를 두고 소통하게 된다. 이 결과로 Policy 는 Mechanism Layer에 의존성을 갖지 않는다.
![](https://i.imgur.com/Eyjs7bB.png)

DIP 는 인터페이스의 이름을 implementation 을 공급하는 측이 아닌 Client측의 이름을 붙이도록 권고한다.