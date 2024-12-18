---
상위 링크: "[[Creational Pattern]]"
---
# Factory Method Pattern
## Purpose
> Factory Method Pattern exposes a method for creating objects, allowing subclasses to control the actual creation process.

Factory Method Pattern은 **상속**을 사용하여 구체적인 객체 생성 로직을 서브클래스에서 구현하도록 한다.

Factory Method Pattern은 어떠한 클래스가 작성되는 시점에 해당 클래스에서 특정한 인스턴스를 생성하는 것을 미룬다. 이후 미뤘던 과정은 서브클래스에 의해서 구체적인 로직으로 작성된다. 

## When to Use
1. 클래스가 필요로하는 객체가 무엇인지 알 수 없을 때
2. 서브클래스만이 어떤 객체를 생성될지 명세할 수 있을 때
3. 수퍼클래스가 서브클래스에게 객체 생성 과정을 미루고 싶을 때

## Participant

* Creator와 Product를 포함하는 Abstract한 부분은 공통 기능을 정의한다.
* ConcreteCreator에서는 어떤 ConcreteProduct를 사용할 지 결정하고 생성한다.
* Creator와 Product는 ConcreteCreator나 ConcreteProduct에 대해서는 알지 못한다.
## Class Diagram

![](https://i.imgur.com/T7RqdTd.png)

