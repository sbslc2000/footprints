# Observer Pattern

## Purpose

> Let one or more objects be notified of state changes in other objects within the system

Observer Pattern은 하나의 객체의 상태가 변경될 때 다른 객체들이 이를 알아차리고 어떠한 액션을 취하게 만들고 싶을 때 사용한다. 

객체간의 일대다 관계를 형성하며, 일 측은 다측에 대해 알림을 위한 인터페이스 레벨로만 의존하여 양 측간 Loose coupling이 적용된다.

## Participant
* Subject : 일 측의 객체로, 상태를 갖는다.
* Observer : 다 측의 객체들로, subject의 상태 변경을 알아차리기를 원하는 객체들이다.

## Class Diagram

![](https://i.imgur.com/j8CHXLf.png)

## Example Source Code
[design-pattern-example/src/main/java/io/github/sbslc2000/observer at main · sbslc2000/design-pattern-example · GitHub](https://github.com/sbslc2000/design-pattern-example/tree/main/src/main/java/io/github/sbslc2000/observer)