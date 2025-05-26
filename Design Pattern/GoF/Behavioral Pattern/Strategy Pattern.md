---
상위 링크: "[[Behavioral Pattern]]"
---
# Strategy Pattern
## Purpose 
> Strategy pattern defines a family of algorithms, encapsulates each one and make them interchangeable. Strategy pattern lets the algorithms vary independently from clients that use it

전략 패턴은 알고리즘의 군을 정의하고, 각각을 캡슐화하여 상호교체 가능하게 만든다. 전략 패턴을 사용하면 알고리즘을 사용하는 클라이언트에게는 영향을 주지 않으면서 다른 알고리즘을 적용시킬 수 있다.

## When to Use
1. 연관된 클래스들의 차이가 오직 행동(알고리즘)밖에 없을 때
2. 다양한 버전의 알고리즘들이 필요할 때
3. 알고리즘이 런타임에 결정되어야 할 때
4. 조건문이 매우 복잡하고 유지보수하기 어려울 때

## Solution
* 서로 다른 클래스에서 사용하는 알고리즘을 식별하고 분류해라.
* 이들을 표현할 수 있는 공통 인터페이스를 만들고 각각의 알고리즘을 구현해라
* 클라이언트 오브젝트가 구체적인 알고리즘을 주입받아 사용하게 만들어라
* 이 때 클라이언트 클래스는 인터페이스만을 의존해야 한다!

## Class Diagram

![](https://i.imgur.com/eVvkSUv.png)


![]()
