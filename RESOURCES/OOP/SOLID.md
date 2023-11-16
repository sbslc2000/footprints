e# SOLID Design Principle

SOLID 라는 martin이 정리한 디자인 원칙에 대해 살펴볼 것이다

# Hierarchy of Pattern Knowledge

![[Pasted image 20230920234110.png]]
4-2 때 배우는 설계패턴에서는 23개의 디자인 패턴을 정의하고 있는데, 이 중 하나는 Strategy Pattern이다.

Strategy Pattern이란 알고리즘을 군집화하여 이것을 encapsulation하여 바꿔낄수있게 만드는 것이다. 이를 통해 클라이언트가 사용하는 알고리즘들이 클라이언트와 별개로, 독립적으로 변화할 수 있게 된다. = 알고리즘의 변화가 클라이언트에 영향을 미치지 않는다.

하지만 Design Pattern만을 공부하는 것으로는 설계의 근간이 변화할때 대응하기 어렵다. 따라서 이것을 support하는 지식인 객체지향설계원칙에 대해 알아야 한다.

객체지향 설계원칙에는 “변하는 것들을 encapsulate시켜라, composition을 inheritance보다 선호하라, 구현이 아닌 설계에 기반한 프로그램을 만들어라”와 같은 것들이 있다. 이러한 설계원칙을 이해하기 위해서는 객체지향의 개념에 대해 알 필요가 있다.

# Design Smells

설계원칙은 왜 필요한가? → 설계 악취

설계 악취란 소프트웨어나 시스템이 잘못 design됐을 때 나타나는 신호, 혹은 그런것을 느끼게 해주는 증상을 의미한다.

Design smells = **various signs and symptoms** of bad design

- **Rigidity** : 시스템을 변경하기 어려운 것. 어떤 변경을 하려했더니, 시스템의 다른 부분까지 영향을 미쳐 변경해야하는 것(Change Propagation이 과도하게 일어나는 것)
- **Fragility** : 설계가 망가지기 쉬운것. 코드가 잘못되기가 쉬운 것. 한 군데를 변경했는데, 이것이 다른 부분이 잘못되는데에 원인이 되는 것.
- **Immobility**: 모듈이 재사용하기 어려운 상태. 모듈을 분리하는데에 수고와 어려움이 존재
- **Viscosity**: 설계를 유지하면서 어떤 시스템을 변경하는게 매우 어려운 상태. 정공법보다 꼼수를 사용해야하는 상황이 많이 발생하는 상태.
- **Needless Complexity**: 과도한 설계. 설계가 유용하지 않은 부분을 포함하여 불필요하게 복잡한 것
- **Needless Repetition** : copy-and-paste 가 반복되어, 코드가 중복하여 나타나는 것
- **Opacity** : 만든 사람의 의도를 파악하기 어려운 상태.

![[Pasted image 20230920234124.png]]
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

# Single Responsibility Principle (SRP)

소프트웨어는 마치 맥가이버칼과 같아야하나?

> **A Class should have one, and only one, reason to change**

클래스(모듈)은 단 하나의 변경될 이유만 가져야한다.

이 변경될 이유는 responsibility로부터 결정되어야한다. → 각각의 모듈은 하나의 responsibility만 가져야한다.

## Concept

Responsibility란 클래스의 의무이다. R.C. Martin은 Responsibility를 클래스를 변경할 요인으로 봤다.

→ 클래스가 많은 Responsibility를 갖는다면, 자주 변경될 것이다. 자주 변경된다면, 버그를 만들 확률이 높아질 것이며, 변경에 대해 다른 클래스들도 영향을 미칠 것이며, 그는 또다른 버그를 만들어낼 지도 모른다.

따라서 클래스는 적은 책임을 가져야 하고, 이상적으로는 한 종류의 책임만 가져야 한다.

- **Responsibility**
    - “a contract or obligation of a class”
    - **reason to change**
        - More responsibilities == More likelihood of change
        - **The more a class changes, the more likely we will introduce bugs**
        - Changes can impact the others

여러 reponsibilities가 한 클래스에 묶여있다면, 개별적인 클래스로 분리하라.

이것을 측정하기 위해서는 **Cohesion**을 봐야한다.

Cohesion: 하나의 클래스 혹은 모듈이 강하게 관련된 것들의 책임만들 갖고 있는가

하지만 이런것을 극도로 따른다고 해서 디자인이 좋아지는 것은 아니다. 각각의 principle을 너무 과도하게 지키다 보면 더 안좋은 설계가 될 수도 있다. → 클래스의 개수가 많아져 복잡해지고, 관리하고 이해하기 어려워질 수 있다

- **SRP: Separate coupled responsibilities into separate classes**
    - Related measure – **Cohesion**: how strongly-related and focused are the various responsibilities of a module

## Example of SRP violation: Case 1

학생을 sorting 하는 responsibility 를 어디에 부여해야할까?

GRASP에 의하면, sorting을 할 때에 필요한 데이터가 있는 곳에 reponsibility를 부여하는 것이 좋을 수 있다. 이 경우 Student에 Comparable interface를 implement하여 sorting 에 대한 책임을 부여할 수 있다.

이경우는 SRP를 만족하는가?

- Often we need to sort students by their name, or SSN. So one may make Class Student implement the Java Comparable interface.

```java
class Student implements Comparable {
… int compareTo(Object o) { … } …
}
```

SRP는 business entity이고, Student의 client가 sorting의 requirement가 있다고 해서 Student가 가져야한다는 법은 없다.

Student 가 sorting의 책임을 지니게 되면, sorting이 변화할때마다 student를 갖고 사용하는 class들이 모두 recompile되고 테스트되어야한다.

이러한 문제는 Student 라는 클래스가 두가지 다른 responsibility를 bundled하고 있기 때문이다. ( business entity + sorting) 따라서 이러한 설계는 SRP의 위반이다.

- Student is a **business entity**, it does not know in what order it should be sorted since the order of sorting is imposed by the client of Student.
- Worse: every time students need to be ordered differently, we have to recompile Student and all its client.
- Cause of the problems: **we bundled two separate responsibilities** (i.e., student as a business entity with ordering) into one class – a violation of SRP
![[Pasted image 20230920234209.png]]

이 경우 Student가 변경되는 것에 대해 Register과 AClient가 영향을 받게 된다.

Register 는 Student의 sorting에 대한 내용을 사용하지도 않지만, sorting에 대해 알게 된다. (change 의 대상이 된다.)

→ comopareTo 에 대한 책임을 분리해야하지 않나

사실 sorting이란 기능은 AClient가 원하던 기능이다.

### Example of design following SRP

![[Pasted image 20230920234220.png]]

Student class는 그대로 놔두기로 한다. 이 경우 Register 혹은 Student를 알고있는 다른 클래스들은 Recompile 되거나 testing 될 필요가 없다.

개선된 design에서는 AClient가 sorting하는 기능을 두기 위해 SortStudentBySSN이라는 클래스를 만든다. 이 클래스는 주민등록번호로 Student를 정렬할 수 있는 기능을 갖고있고, Comparator를 implement한다.

SRP에 의하면 새로운 기능은 새로운 클래스에 할당해야한다. AClient는 SortStudentBySSN을 사용하게 된다. SortStudentBySSN의 변경은 AClient에게 영향을 준다. 하지만 이것은 불필요한 change propagation이 아니다.

SortStudentBySSN은 Student에 의존한다. SortStudentBySSN의 변경은 Student에는 영향을 주지 않는다.

이를 확장하여 만약 Name을 기준으로 sorting해야하는 requirement 가 발생하는 경우, 이 책임을 구현할 새로운 클래스를 만들 수 있다.

## Example of SRP violation: Case 2

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dd27613-a3b7-447c-836c-78e693749fe6/Untitled.png)

이 경우 responsibilities 가 두개가 혼재해있기 때문에 문제가 발생한다. 만약 area() 의 시그니처가 변화한다면, Rectangle 클래스를 변화시켜야하고, 이로인해 CGA 와 GA가 recompile되어야 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/37cdf07e-6647-45ee-94df-1de64a50b1ef/Untitled.png)

### Example of design following SRP (not enough yet)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7bbcc51b-e772-4dec-a640-19f91e0733a2/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f916310-ee94-4a6b-b520-5dce9085438f/Untitled.png)

### Example of design following SRP

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c970167-47fa-4dcf-84d5-ea6fac25c320/Untitled.png)

이 그림은 SRP의 적용 결과이다.

draw의 책임은 GraphicRactangle이 갖도록한다.

반면 CGA의 부분은 분리하기 어려워보인다. area라는 메서드는 length와 width를 필요로하기 때문이다.

이러한 설계를 통해, 만약 GA의 변화로 인해 Graphic Rectangle의 draw가 변화해야한다고 하자. 이 경우 Graphic Rectangle의 변화는 GA까지 영향을 미친다. 반면 Geometric Rectangle로는 change propagation 이 파급되지 않는다. 따라서 조금더 개선할 필요가 있다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8704d448-85a3-4e02-a103-753a14900ef3/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed2e199a-4d8b-4713-afa6-d875522faf7a/Untitled.png)

Rectangle 을 추상화하여 abstract class로 만들고, Geometric Rectangle과 Graphic Rectangle을 이를 상속하게 한다.

이런 구조로는 변경의 여파가 양측에 영향을 끼치지 않는다.

## Identifying Responsibility Can Be Tricky

SRP를 적용할 때, 미묘한 문제들을 맞닥뜨릴 수 있다.

초기에는 그것이 같은 종류의 responsibility로 판단이 됐지만, 확장하다보니 알고보니 서로 다른 종류의 repsonsibility라는걸 나중에 깨달을 수 있다.

- Responsibility (in SRP)
    - A reason for change
    - Note: **sometimes hard to see multiple responsibilities**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a90f52d-90af-4731-bdd7-285ac8a0aabd/Untitled.png)

이러한 내용들은 Modem이 가져야하는 행동으로 하나의 Responsibility라고 생각할 수 있다. 하지만 이와같이 system 을 design하고 나서, 이후 확장시 다음과 같은 일이 벌어질 수도 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f7703fc-ef08-4fe4-bbb1-21aa42883e95/Untitled.png)

어떠한 모뎀은 dial과 hangup을 가질 필요가 없는 경우가 있다. 따라서 이 메서드들은 서로 다른 책임을 구현하는 것이라는걸 알게 되었다

- Two responsibilities
    - Connection management
    - Data communication

요구사항에 따라서 문제가 앞으로 어떻게 진화할 것이냐에 대한 통찰을 하는 것, 그를 통해 동종의 responsibility와 서로다른 responsibility를 구분하는 것이 필요하다.

- **Lesson: It depends on how the application is changing!**

불필요한 complexity가 생기는 것을 피하라!

DedicatedModem이 생길지 안생길지도 모르는데 미리 분리할 필요는 없다.

- **Beware: Avoid Needless Complexity**
    - **If there is no symptom, it is not wise to apply the SRP or any other principle!**

# Open Closed Principle (OCP)

개방폐쇄 원칙

> **Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.**

소프트웨어 엔티티는 확장에 대해서 열려있어야하고 수정에 대해서 닫혀있어야 한다.

_You should be able to extend a class’s behavior, without modifying it._

어떤 클래스의 behavior를 확장할 때 기존의 시스템을 과도하게 변경하는일이 없어야한다.

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

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0e43f43-bf15-4b0f-a985-5a51e054f3da/Untitled.png)

## Problems of Bad Design

만약 employee 의 타입이 하나 증가한다면, 해당 employee의 메서드를 구현해야할 뿐만 아니라 incAll의 메서드도 추가해주어야한다. (extension에 대해서 기존의 code가 close 되어있지 않다) 이러한 클라이언트가 많다면 굉장히 여러부분을 수정해주어야 할 것이다.

- **Rigid**
    - Adding new employee type **requires significant changes**

또한 , 수많은 condition statement 는 코드가 오류가 발생하기 쉬운 구조이다.

- **Fragile**
    - Many **switch/case or if/else statements**
    - Hard to find and understand

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24779339-2260-4e12-9d66-dbc7914d5cf7/Untitled.png)

incAll이라는 메서드를 다른 프로젝트에 재사용하려할 때, 해당 시스템에 Secretary가 존재하지 않는다면 해당 코드를 지워줘야할 것이다.

- **Immobile**
    - To reuse incAll( ) → we need Faculty, Staff, Secretary, too!
    - **What if we need just Faculty and Staff only?**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/923d416a-2d00-449c-9778-26caccb305dc/Untitled.png)

## Better Design

이 문제를 해결하기 위해서는, Employee라는 상위 클래스에서 incSalary()라는 것을 놓고, 이를 상속한 엔티티들이 이를 구현할 수 있게 해주어야한다.

이런 경우 확장이 일어날 때 client code의 수정이 발생하지 않는다. → modification의 close

OCP를 잘 지키기 위해서는 polymorphism과 같은 객체지향 테크닉이 활용이 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4006ca98-8468-4a7d-9ff0-110fde70f621/Untitled.png)

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

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0432261e-c7d5-4c16-a380-1a6cf8739872/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1be15347-6e99-49c4-9381-02defe89e5c2/Untitled.png)

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

# Liskov Substitution Principle

리스코프 치환 원칙

> **Subtypes must be substitutable for their base types**

Subtype은 Base types을 대신할 수 있어야 한다.

Liskov 치환원칙은 형식이 아닌 **내용에** 관련된 원칙이다. → Subtype의 객체가 Base Type의 동등한 역할을 하고, 대신해서 쓰일 수 있어야 한다. ↔ 단순히 프로그래밍 레벨에서에 그치는 것이 아니다.

Derived classes must be substitutable for their base classes

Liskov 치환원칙은 inheritance를 사용할지에 대한 여부를 결정하는데 도움을 준다.

- A rule that you want to check **when you decide to use inheritance or not**

P타입의 객체가 C타입의 객체로 대신 쓰였을 때, 우리가 원하는 프로그램의 property가 변경되어서는 안된다.

- If C is a subtype of P, then objects of type P may be replaced with objects of type C **without altering** **any of the desirable properties of the program**

## Subtyping VS Implementation Inheritance

Inheritance는 Subtype을 만들어내는 것이기 때문에 Subtyping 과 Implementation Inheritance를 같은 개념으로 생각하고는 한다. 하지만 이 둘은 분리시킬 수 있는 개념이다.

Subtyping이란 IS_A relationship을 성립시키는 것이며, interface inheritance라고 부른다.

- **Subtyping**
    - establishes an **IS_A relationship**
    - also known as **interface inheritance**

Implementation Inheritance란 구현을 재사용하기 위한 목적으로 행해지는 상속으로, 의미적인 관계를 만들어내지 않기 때문에 IS_A 관계를 성립하지 못한다. 이는 code inheritance라고도 부른다.

- **Implementation Inheritance**
    - only **reuses implementation** and establishes a **syntactic** relationship not necessarily a **semantic** relationship
    - Also known as **code inheritance**

많은 OOP 언어에서는 ‘extends’라는 상속 키워드가 두 Inheritance를 동시에 수행하도록 되어있다.

Liskov 치환 원칙은 엄밀히 말하면 Subtyping의 관계에서 발생할 수 있는 문제이다.

- Most OOP languages like Java, C++, and C#, inheritance keyword such as “extends” does the both Subtyping and Implementation Inheritance
    - But some languages distinguish them

## The Liskov Substitution Principle and Reuse

어떠한 프로젝트에서 List라는 타입의 클래스를 만들었다고 하자.

다음 프로젝트에서는 Queue 라는 자료구조가 필요하다. 이때 List의 기능을 재사용하면 Queue를 구현할 수 있을 것 같다. 이때 List의 모든 implementation의 재사용을 위해 List를 상속한 Queue를 만든다고 했을 때, 이러한 것은 좋은 디자인일까? ㄴㄴ 아니다.

Inheritance를 통해 이 관계에는 **잘못된 Subtyping을 하게 된다.** List 타입이 요구되는 모든 위치에 Queue라는 타입의 인스턴스가 대신 들어가도 잘 동작하나? 그렇지 않을 것이다. Queue 는 List보다 더 제한적인, restricted 된 자료구조이기 때문이다. (중간에 있는 자료 조작 불가)

그럼 코드 재사용은 어떻게 해야할까? Queue라는 클래스가 List를 갖고있으며 활용하면 된다. Queue라는 객체 외부에서 봤을 때는 List와 subtype 관계가 있지 않으면서도, List의 기능을 재사용할 수있다.

- **Think twice when you decide to use Inheritance!**
    - If you want to reuse implementation of List, you had better exploit **object composition, not inheritance**.
    - If you inherit Queue from List, then you **violate LSP** since Queue object cannot be substitutable for List.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/59757116-bff9-41d2-852a-623127125b8c/Untitled.png)

## Violation Example of LSP

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ff60fb82-ea5f-4c92-88a7-7d8230c62393/Untitled.png)

## Violation of LSP may lead to another violation

LSP를 어기면, OCP를 어기는 디자인으로 악화될 수 있다.

- Assume that when CType is passed to f() instead of PType, it causes f to misbehave; it means that CType violates the LSP
    
    → CType is fragile in the presence of f
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b3c93fb-bac7-4b3b-9863-e3a2b2325c2f/Untitled.png)

The owner of f might want to put test code for Ctype

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1db2ff22-dc95-4ed4-90b1-574e3fe024d2/Untitled.png)

The above reaction is worse. Now, it is violating OCP, too! → **f is not closed to all various subtypes of PType**

f는 더 이상 확장에 대해 closed 되어있지 않다. 이것의 근본적인 이유는 잘못된 상속 때문일 것이다. PType 과 CType의 inheritance를 번복해야한다.

# Dependency Inversion Principle

의존성 역전 원칙

> **High-level modules should not depend on low-level modules. Both should depend on abstractions.**

High-level Module은 Low-Level Module에 dependency를 가져서는 안된다. 이 둘은 모두 Abstraction에 dependency를 가져야 한다.

- Abstractions should not depend on details. Details should depend on abstractions.

DIP는 구조적 분석설계방법을 따른다면 나타날 수 있는 dependency의 방향을 반대로 뒤집어 준다.

- Why Inversion?
    - DIP attempts to **“invert” the dependencies** that result from a **structured analysis and design approach**

## Typical in Structured Analysis & Design

구조적 분석설계는 Top-down 설계이다. 가장 상위의 Program이라는 모듈이 있다면, 이것의 기능 분해 Function Decomposition을 통해 여러 모듈로 나뉘어지고, 이 친구들이 각각의 기능을 Function이란 단위로 호출하게 된다.

→ 상위 모듈이 하위 모듈에 dependency를 갖는 구조

→ 자주 변경되는 단위의 function이 변경되면, 이 변경의 여파가 위로 쭉쭉 올라간다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/144cbaab-a185-4a97-980b-a61d1aaf2c14/Untitled.png)

## Dependency Inversion Principle

DIP는 모듈간에 인터페이스를 놓아, 상위 모듈과 하위 모듈이 Abstraction layer를 통해 연결되도록 한다. 이 경우 하위 모듈의 의존방향이 역전이 된다.

이때 Class 1의 변경은 그 여파가 전달되지 않는다.

Interfaces and abstract classes are high-level resources Concrete classes are low-level resources

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32b83edc-8668-4032-8849-90663ca4727e/Untitled.png)

## Inversion of Ownership

DIP 는 Control flow만 역전시키는 것이 아닌 **ownership**도 역전시킨다.

Server 가 Interface 를 implement 하고, Client가 interface를 사용하는 관계에서, DIP는 Client가 interface의 ownership을 갖게 한다. : 사용하는 측의 소유

- Its not just an inversion of **dependency**, DIP also inverts **ownership**
    - Typically a service interface is “owned” or declared by the server, here the client is specifying what they want from the server
    - **DIP → Clients should own the interface!**

Layer 아키텍처에 적용하면 다음과 같다.

이 구조는 구조적 분석설계에서의 레이어 의존 다이어그램이다.

상위 레이어는 하위 레이어를 사용하여 의존관계가 형성된다. 이것은 DIP에 위배되는 상황이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7abfc401-d77b-48a9-a610-af48626fd355/Untitled.png)

DIP를 적용한 구조는 다음과 같다. 각각의 레이어는 다른 레이어와 소통할 때 Abstraction layer를 두고 소통하게 된다. 이 결과로 Policy 는 Mechanism Layer에 의존성을 갖지 않는다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6099a844-4447-411f-99ec-45b5ba1f17bb/Untitled.png)

DIP 는 인터페이스의 이름을 implementation 을 공급하는 측이 아닌 **Client측의 이름을 붙이도록 권고한다.**

# Interface Segregation Principle (ISP)

인터페이스 분리의 원칙

> **Clients should not be forced to depend on methods they do not use**

클라이언트는 자신이 사용하지 않는 메서드에 대해서 dependency를 갖도록 강제되어서는 안된다.

인터페이스는 잘개 조개져서 client specific해져야 한다.

Make fine grained interfaces that are **client specific**

## Fat interface

서로다른 클라이언트에게 하나의 인터페이스만을 만들어 불필요한 coupling을 만드는 것은 불필요한 일이다.

인터페이스를 크게 만드는 것은 좋지 않다.

- **Bundling functions for different clients into one interface** create **unnecessary coupling among the clients**.
    - When one client causes the interface to change, all other clients are forced to recompile. → **Solution: Break the interface into cohesive groups**
    - Responsibility 가 유사한 그룹으로 나누어라
- ISP solves **non-cohesive interfaces**
    - Clients should know only abstract base classes that have cohesive interfaces

## ISP Example: Original Design

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e0d4006-eae2-4754-8eb4-c5f35e0db0ba/Untitled.png)

각각의 Application에서 사용하지 않는 API까지 사용할 수 있게 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac586b54-9a9e-4794-b0db-b9e7e310a1dc/Untitled.png)

## ISP Example: Better Design

ISP를 통해 cohesive 한 단위로 나눠주고, 이를 동시에 implement 한 Student를 만든다.

이제는 Roaster Application은 실제로 사용하는 API만 사용할 수 있게 된다. 즉 다른 부분의 변경에 대해서는 영향을 받지 않게 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/511243dc-3ef6-4759-895f-74fb8ff0e5f6/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e23dc6d-d278-4605-96e9-bd8f06adea4e/Untitled.png)

# Summary

- SOLID
    - Single-Responsibility Principle
    - Open-Closed Principle
    - Liskov Substitution Principle
    - Dependency Inversion Principle
    - Interface Segregation Principle
- Design Principles ▪ Help manage dependency ▪ Better maintainability, flexibility, robustness, and reusability ▪ Abstraction is important ▪ GRASP patterns/principles