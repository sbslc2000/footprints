---
상위 링크: "[[Behavioral Pattern]]"
---
# Chain of Responsibility Pattern
연쇄 책임 패턴으로 번역된다.
![](https://i.imgur.com/KQhW48V.png)

## Purpose
> _Gives more than one object an opportunity to handle a request by linking receiving objects together._

- 여러 객체를 체인 형태로 연결해 순차적으로 요청을 처리할 수 있도록 한다.
- 클라이언트의 요청을 처리할 수 있는 후보가 여러개 있다면
    - 이를 정적으로 결정하지 말고
    - 후보들을 체인으로 구성하여
    - 후보들이 직접 처리하거나 아니라면 다음 후보로 전달하는 방식으로 만들자.

## Use When
- 여러 객체가 요청을 처리할 수 있으며, 특정 객체가 처리해야 할 필요는 없을 때
- 객체의 집합이 요청을 처리할 수 있고, 어떤 객체가 처리할지는 **런타임**에 결정되어야 할 때
- 요청이 처리되지 않아도 괜찮은 경우

## Concept
- 각각의 객체는 체인 내에 존재한다.
    - 객체는 요청을 받는다.
    - 이를 직접 처리하거나, 다음 객체에게 전달한다.
- 주요 포인트
    - 요청을 만드는 객체는 어떤 객체가 요청을 처리할지에 대해서 모른다.
    - 요청은 암시적인 수신자를 대상으로 한다.

## Participants
- **Handler**
    - 요청을 처리하는 인터페이스를 정의한다.
    - 선택적으로, 이들은 후속 처리자 링크를 구현한다.
- **Concrete Handler**
    - 해당 객체가 요청을 해결하는 방법을 정의한다.
    - 후속 처리자에 접근할 수 있다.
    - 만약 객체 스스로 요청을 처리할 수 있다면 처리하고, 그렇지 않다면 후속 처리자에게 요청을 전달_forward_한다.
- **Client**
    - 체인 내의 최초의 Concrete Handler에게 요청을 보낸다.

## Class Diagram
체인에 속해있는 각각의 객체는 요청을 처리하기 위한 공통 인터페이스를 공유하며, 체인의 후속 처리자에 접근한다. _shares a common interface for handling requests and accessing its successor_

![](https://i.imgur.com/6jgLvV6.png)

## Consequences
- Benefits
    - 요청을 보내는 객체와 받는 객체의 결합도를 낮춘다.
    - 유연성
    - 요청자는 어떤 객체가 이를 처리하는지 알지 않아도 된다.
- 잠재적인 결함
    - 클라이언트는 어떤 객체가 이를 처리하게 할지 명시할 수 없다.
    - 요청이 처리될 것이라는 보장이 없다

## Related Pattern
- 책임 연쇄(Chain of Responsibility)는 종종 **컴포지트(Composite)**와 함께 적용된다.
 ![](https://i.imgur.com/7390emk.png)

- **책임 연쇄(Chain of Responsibility)**, **커맨드(Command)**, **미디에이터(Mediator)**, **옵저버(Observer)** 패턴은 발신자와 수신자를 분리하는 방법을 다루지만, 서로 다른 트레이드오프를 가진다.
- **책임 연쇄**는 발신자의 요청을 잠재적인 수신자들의 체인을 따라 전달한다.
- **책임 연쇄**는 요청을 객체로 표현하기 위해 **커맨드** 패턴을 사용할 수 있다.
## Example Source Code
[design-pattern-example/src/main/java/io/github/sbslc2000/cor at main · sbslc2000/design-pattern-example · GitHub](https://github.com/sbslc2000/design-pattern-example/tree/main/src/main/java/io/github/sbslc2000/cor)