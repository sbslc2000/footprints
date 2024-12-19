---
상위 링크: "[[Behavioral Pattern]]"
---
# Template Method Pattern
## Purpose
> Identifies the framework of an algorithm, allowing implementing classes to define the actual behavior

알고리즘의 프레임워크를 정의하여, 실제 행동을 자식 클래스에서 구현하게 만든다. 실제 행동은 서브클래스가 구현하며, 수퍼클래스는 골격만을 정의한다.

## Use When
- 알고리즘의 단일 추상 구현이 필요할 때.
- 하위 클래스 간의 공통 동작을 공통 클래스에 집중시켜야 할 때.
- 상위 클래스가 하위 클래스의 동작을 일관되게 호출할 수 있어야 할 때.
- 대부분 또는 모든 하위 클래스가 해당 동작을 구현해야 할 때.

## Concept
* 알고리즘을 템플릿으로 만들어 추상화한다.
	* 알고리즘의 골격은 여러 step의 집합으로 정의된다.
* 몇몇 메서드들은 서브클래스들로부터 반드시 정의될 수 있도록 추상메서드로 정의한다.
* 서브클래스는 알고리즘의 몇몇 단계를 재정의할수는 있지만, 알고리즘의 구조 자체를 바꾸지는 못한다.
* 몇몇 step들은 상위클래스에 정의되어 중복을 제거할 수 있다.

## Class Diagram
![](https://i.imgur.com/TXJIAah.png)
