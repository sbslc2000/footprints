---
상위 링크: "[[Creational Pattern]]"
---
# Abstract Factory Pattern

## Purpose
> provide an interface that delegates creation calls to one or more concrete classes in order to deliver specific objects

Abstract Factory Pattern 관련성 있는 객체들의 군(군집, 계열)을 생성하기 위한 인터페이스를 제공하며, 구체적인 클래스에 의존하지 않고 객체를 생성할 수 있도록 하는 것이다. 이 패턴을 통해 객체 생성의 책임을 특정 클래스가 아닌 팩토리 클래스에 위임함으로써, 코드의 유연성과 확장성을 높일 수 있다.

## Use When
* 시스템이 자신이 사용하는 객체의 생성과정을 모르게 하고 싶을 때
* 시스템이 어떠한 객체들의 군을 사용해야할 때
	* 이러한 객체들은 특정한 테마를 공유하며 함께 사용될 때
* 구현의 세부사항을 노출하지 않으면서도 라이브러리를 공유하고 싶을 때
* 구체 클래스를 클라이언트로부터 분리시키고자 할 때

## Factory Method Pattern과의 차이점

Factory Method Pattern은 **클래스의 스코프**를 다루며, 상속을 통해 객체 생성 메서드를 오버라이딩하여 특정 객체를 생성하도록 열어준다. 이를 통해 객체 생성의 구체적인 내용을 하위 클래스에 위임한다.

반면, Abstract Factory Pattern은 **객체의 스코프**를 다루며, 객체의 조합(composition)과 위임(delegation)을 통해 객체를 생성하는 팩토리 객체를 주입받아 사용한다. 이 패턴에서는 관련된 객체들을 그룹으로 묶어 일관성 있게 생성할 수 있으며, 사용자는 구체적인 클래스에 의존하지 않는다.

## Class Diagram

![](https://i.imgur.com/7KIAhBk.png)

* 클라이언트는 AbstractFactory와 AbstractProduct만 이용한다
* ConcreteFactory는 AbstractProduct 를 구현한 객체 군을 생성한 뒤 반환한다.
* Client는 주입되는 ConcreteFactory만 바뀐다면 완전히 다른 객체 군을 사용할 수 있다. 