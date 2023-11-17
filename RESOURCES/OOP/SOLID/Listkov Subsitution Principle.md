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
    - 
![](https://i.imgur.com/6dOiTMY.png)

## Violation Example of LSP

![](https://i.imgur.com/765yg95.png)
## Violation of LSP may lead to another violation

LSP를 어기면, OCP를 어기는 디자인으로 악화될 수 있다.

- Assume that when CType is passed to f() instead of PType, it causes f to misbehave; it means that CType violates the LSP
    
    → CType is fragile in the presence of f
    
![](https://i.imgur.com/yTwe9w3.png)

The owner of f might want to put test code for Ctype
![](https://i.imgur.com/UcbCdmy.png)

The above reaction is worse. Now, it is violating OCP, too! → **f is not closed to all various subtypes of PType**

f는 더 이상 확장에 대해 closed 되어있지 않다. 이것의 근본적인 이유는 잘못된 상속 때문일 것이다. PType 과 CType의 inheritance를 번복해야한다.
