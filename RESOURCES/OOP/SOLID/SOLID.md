# SOLID Design Principle

SOLID 라는 martin이 정리한 디자인 원칙에 대해 살펴볼 것이다

# Hierarchy of Pattern Knowledge

![[Pasted image 20230920234110.png]]
4-2 때 배우는 설계패턴에서는 23개의 디자인 패턴을 정의하고 있는데, 이 중 하나는 Strategy Pattern이다.

Strategy Pattern이란 알고리즘을 군집화하여 이것을 encapsulation하여 바꿔낄수있게 만드는 것이다. 이를 통해 클라이언트가 사용하는 알고리즘들이 클라이언트와 별개로, 독립적으로 변화할 수 있게 된다. = 알고리즘의 변화가 클라이언트에 영향을 미치지 않는다.

하지만 Design Pattern만을 공부하는 것으로는 설계의 근간이 변화할때 대응하기 어렵다. 따라서 이것을 support하는 지식인 객체지향설계원칙에 대해 알아야 한다.

객체지향 설계원칙에는 “변하는 것들을 encapsulate시켜라, composition을 inheritance보다 선호하라, 구현이 아닌 설계에 기반한 프로그램을 만들어라”와 같은 것들이 있다. 이러한 설계원칙을 이해하기 위해서는 객체지향의 개념에 대해 알 필요가 있다.

# Design Smells
SOLID가 지켜지지 않았을 때는 [[Design Smell]] (설계 악취) 가 난다고 표현한다.

# Dependency Management

많은 경우 dependency의 관리가 안되고 있을 때 design smell이 난다.

- Design smells are resulted from **mismanaged dependencies**

dependency가 관리가 안된다면 높은 수준의 coupling이 발생하여 스파게티코드가 된다.

- Mismanaged dependencies
    - **Tangled mass of couplings (spaghetti code)**

객체지향 언어들은 dependency를 관리하기 위한 여러 장치를 제공한다.
인터페이스를 통해 dependency 방향을 없애거나 끊을 수 있다.
Polymorphism을 통해 dynamic하게 메서드를 호출할 수 있는 장치를 제공한다.
이것들을 어떤 형태로 사용해야하는가? → SOLID
solid를 통해 dependency를 관리하고 설계 악취가 발생하지 않게 할 수 있다.

- OO languages provide tools helping managing dependencies
    - **Interfaces**: **break or invert the direction of certain dependencies**
    - **Polymorphism**: allows **modules** to invoke methods **dynamically**
    - Lots of power to shape the dependencies, indeed
- So, how do we want them shaped?

# Object-Oriented Design Principles

객체지향 설계 원칙에는 여러가지가 있고, GRASP에 대해서는 이미 알아보았다.

- **SOLID principles** by R.C. Martin
- GRASP patterns/principles by C. Larman
- **Program to interfaces, not implementations**
- **Favor object composition over class inheritance**
- **Encapsulate what varies**

## R.C. Martin’s Software design principles (SOLID)

- The Single-Responsibility Principle (SRP)
- The Open-Closed Principle (OCP)
- The Liskov Substitution Principle (LSP)
- The Interface Segregation Principle (ISP)
- The Dependency Inversion Principle (DIP)

# SOLID 주요 내용

[[Single Responsiblity Principle]]

[[Open Closed Principle]]

[[Listkov Subsitution Principle]]

[[Dependency Inversion Principle]]

[[Interface Segregation Principle]]

# Summary

- SOLID
    - Single-Responsibility Principle
    - Open-Closed Principle
    - Liskov Substitution Principle
    - Dependency Inversion Principle
    - Interface Segregation Principle
- Design Principles ▪ Help manage dependency ▪ Better maintainability, flexibility, robustness, and reusability ▪ Abstraction is important ▪ GRASP patterns/principles