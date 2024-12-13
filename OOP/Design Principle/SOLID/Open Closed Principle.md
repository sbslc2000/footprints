---
상위 개념: "[[SOLID]]"
---
# Open Closed Principle (OCP)

개방폐쇄 원칙

> **Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.**

소프트웨어 엔티티는 확장에 대해서 열려있어야하고 수정에 대해서 닫혀있어야 한다.

_You should be able to extend a class’s behavior, without modifying it._

어떤 클래스의 behavior를 확장할 때 기존의 시스템을 과도하게 변경하는일이 없어야한다. 기존의 코드를 수정하기보다는 반드시 새로운 코드를 추가하는 방식으로 시스템의 행위를 변경할 수 있도록 설계해야만 소프트웨어 시스템을 쉽게 변경할 수 있다는 것이 이 원칙의 요지이다.

## Conforming to OCP

확장에 대해 열려있어야한다.

확장에 대해서는 당연히 code change가 추가되어야한다. 하지만 이것에 의해서 이미 존재하던 다른 코드들이 같이 변경이 된다면 좋은 확장성을 갖고 있는 시스템이라고 이야기할수 없다.

- **Open for extension**
    - Behavior of the module can be extended
    - We are able to change what the module does

확장에 의해서 기존의 코드의 변경이 생기지 않도록 설계해야한다.

- **Closed for modification**
    - Extending behavior **does not result in excessive modification such as architectural changes of the module**

OCP를 지키지 못하면 Rigidity의 악취가 나게 된다. → 변경사항의 끝없는 전파

- Violation Indicator: Design Smell of **Rigidity**
    - A single change to a program results in a **cascade of changes to dependent modules**

## Bad Design Example

HRMgr의 incAll은 salary를 올려주는 메서드이다. 이를 구현하기 위해 모든 employee에 대해 for문을 돌면서 각각의 type에 해당하는 salary를 올려주는 메서드를 호출해주고 있다.

이러한 design은 OCP를 잘 지키지 못하는 설계이다.

![](https://i.imgur.com/ihT9VNq.png)

## Problems of Bad Design

만약 employee 의 타입이 하나 증가한다면, 해당 employee의 메서드를 구현해야할 뿐만 아니라 incAll의 메서드도 추가해주어야한다. (extension에 대해서 기존의 code가 close 되어있지 않다) 이러한 클라이언트가 많다면 굉장히 여러부분을 수정해주어야 할 것이다.

- **Rigid**
    - Adding new employee type **requires significant changes**

또한 , 수많은 condition statement 는 코드가 오류가 발생하기 쉬운 구조이다.

- **Fragile**
    - Many **switch/case or if/else statements**
    - Hard to find and understand
![](https://i.imgur.com/8e2hYqA.png)

incAll이라는 메서드를 다른 프로젝트에 재사용하려할 때, 해당 시스템에 Secretary가 존재하지 않는다면 해당 코드를 지워줘야할 것이다.

- **Immobile**
    - To reuse incAll( ) → we need Faculty, Staff, Secretary, too!
    - **What if we need just Faculty and Staff only?**
![](https://i.imgur.com/RCwy19k.png)

## Better Design

이 문제를 해결하기 위해서는, Employee라는 상위 클래스에서 incSalary()라는 것을 놓고, 이를 상속한 엔티티들이 이를 구현할 수 있게 해주어야한다.

이런 경우 확장이 일어날 때 client code의 수정이 발생하지 않는다. → modification의 close

OCP를 잘 지키기 위해서는 polymorphism과 같은 객체지향 테크닉이 활용이 된다.

![](https://i.imgur.com/gVGCgFj.png)

- **When Engineer is added, incAll() does not even need to recompile.**
- This design is open to extension, closed for modification.

## Abstraction is the Key!

OCP는 Abstraction을 강조한다.

Abstraction이란 잘 변하지 않고, 고정되어 있는 성질의 것이다.

- **Abstractions**
    - **Fixed** and yet represent **an unbounded group of** **possible behaviors**
    - Abstract base class : fixed
    - All the possible derived classes : unbounded group of possible behaviors

프로그래밍을 할때 implementation이 아닌 interface를 기반으로 해야한다.

- Program the class
    - **to interfaces (or abstract classes)**
    - not to implementation (concrete classes)

Client 가 concrete 한 Server class를 사용하는 것은 OCP입장에서 좋지않다. Server class의 내용의 변경이 Client에 영향을 주기 때문이다.

이를 해결하기 위해서 중간에 추상화 수준이 높은 인터페이스를 놓는다면 Server의 변경에 Client가 영향을 받지 않는다.
![](https://i.imgur.com/ZA2yLxO.png)
![](https://i.imgur.com/dFPw2Km.png)

## Anticipating Future Changes

OCP를 잘 지킨다는 것은 미래에 일어날 변화를 예측하는 것이다.

OCP를 맹목적으로 지킨다면 needless complexity가 발생할 수 있다. 따라서 어떤 종류의 변화가 일어날지 파악해야하고, 자주 바뀌는 변화가 생긴다면 이것을 OCP를 지키게 해야한다.

하지만 자주 일어나는 변화가 아니라면, OCP를 적용하는 것은 불필요할지도 모른다. OCP는 expensive한 time과 effort를 요구한다.

- Strategy is needed
    - **Choose the kinds of changes** against which to close design
    - Guess the **most likely kinds of changes**, and then **construct abstractions** to protect him from those changes.
- Beware: Consider the cost!
    - **Conforming to OCP is expensive**
    - **Time and effort** to create appropriate abstractions
    - Abstractions also **increase complexity**

한번은 속인사람 잘못, 두번째 이후부터는 네 잘못

OCP를 미리 적용할게 아니라, 일단 안생길거라고 가정하고 만들고, 만약 변화가 감지가 된다면 추상화 장치를 만들어라. → 일어나지도 않은 일들에 대해서 미리 고려하지 마라

- **Do not put hooks in for changes that might happen**
    - "Fool me once, shame on you. Fool me twice, shame on me."
    - Initially write the code **expecting it to not change**.
    - When a **change occurs**, implement the abstractions that protect from future changes of that kind.
    - It's better to take the first hit as early as possible.
        - We want to know what kind of changes are likely before going too far in the development.
    - Use TDD and listen to the tests.
