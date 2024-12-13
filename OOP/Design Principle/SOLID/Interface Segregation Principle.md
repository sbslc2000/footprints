---
상위 개념: "[[SOLID]]"
---
# Interface Segregation Principle (ISP)

인터페이스 분리의 원칙

> **Clients should not be forced to depend on methods they do not use**

클라이언트는 자신이 사용하지 않는 메서드에 대해서 dependency를 갖도록 강제되어서는 안된다.

인터페이스는 잘개 조개져서 client specific해져야 한다.asdf

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
![](https://i.imgur.com/1iSCzqf.png)

각각의 Application에서 사용하지 않는 API까지 사용할 수 있게 된다.
![](https://i.imgur.com/A54BoWl.png)

## ISP Example: Better Design

ISP를 통해 cohesive 한 단위로 나눠주고, 이를 동시에 implement 한 Student를 만든다.

이제는 Roaster Application은 실제로 사용하는 API만 사용할 수 있게 된다. 즉 다른 부분의 변경에 대해서는 영향을 받지 않게 된다.
![](https://i.imgur.com/Iov2XRy.png)
![](https://i.imgur.com/nT1UcHr.png)
