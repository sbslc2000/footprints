---
상위 링크: "[[Behavioral Pattern]]"
---
# State Pattern
## Purpose
> Ties object circumstances to its behavior, allowing the object to behave in different ways based upon its internal state

> 상태를 기반으로한 행동과 전이 로직의 캡슐화

객체의 상태를 행동과 연결하여, 객체가 내부 상태에 따라 다른 방식으로 동작할 수 있도록 한다. 

## Use When
* 객체의 행동이 상태에 영향을 받을 때
* 객체의 행동과 상태가 복잡한 조건으로 묶여 있을 때
* 상태 변환이 명시적일 필요가 있을 때

## Class Diagram
![](https://i.imgur.com/WnddLha.png)

## Applying Step
1. 상태를 추상화한 상태 인터페이스를 만들어라.
	1. 이는 Context 가 수행할 수 있는 동작들을 선언해야한다.
2. Context의 상태에 따른 ConcreteState 들을 구현하라.
	1. 이는 특정한 상태에서의 행동들을 구현한다.
	2. 상태 전이 로직이 발생할 수 있다.
3. Context 객체의 동작을 State 구현체에 위임하라.
