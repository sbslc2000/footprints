---
상위 링크: "[[Bridge Pattern]]"
---
# Bridge Pattern
## Purpose
> Defines an abstract object structure independently of the implementation object structure in order to limit coupling

구현 객체 구조와 독립적으로 추상 객체 구조를 정의하여 결합도를 제한한다.

## Use When
- 추상화와 구현이 컴파일 시점에 결합되지 않아야 할 때.
- 추상화와 구현이 독립적으로 확장 가능해야 할 때.
- 구현 세부 사항을 클라이언트로부터 숨겨야 할 때.

## Participant
* Abstraction
* RefinedAbstraction
* Implementor
* ConcreteImplementor

## Class Diagram
![](https://i.imgur.com/PmAB4rL.png)
- Abstraction (그리고 RefinedAbstraction)은 엔티티들에 대한 고수준의 컨트롤 계층*high-level control layer*이다.
- 이 계층은 그 자체로는 아무런 일도 하지 않도록 의도되었다.
- 이들은 구현 계층에 모든 일을 위임한다. _It should delegate the work to the implementation layer (also called platform)_
- 여기서 Abstraction은 사실 추상 클래스와는 관련이 없다.
    - 개념적인 것을 다루는 의미에서 사용됨

## Comparison with Adapter
- 유사점
    - 두 패턴 모두 기본 구현의 세부사항을 숨기기 위해 사용한다.
- 차이점
    - 어댑터 패턴은 관련이 없는 구성 요소들을 함께 작동하도록 만드는데에 초점이 맞춰져 있다.
        - 설계 이후 시스템_after they’re designed_에 적용된다. (재설계_reengineering_, 인터페이스 엔지니어링)
    - 브릿지 패턴은 설계 초기 단계_up-front in a design_에서 추상화와 구현을 독립적으로 변화시킬 수 있도록 사용된다.
        - 확장 가능한 시스템 설계를 목표로 한다.
        - 분석이나 시스템 설계 시점에서 알려지지 않은 새로운 객체 유형_beast_도 추가할 수 있다.
    - 구조적 차이
        - 브릿지 패턴은 복잡한 엔티티를 그 구현으로부터 추상화할 수 있다.
        - 어댑터 패턴은 단일 인터페이스에만 추상화한다.