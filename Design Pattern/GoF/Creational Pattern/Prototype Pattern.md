---
상위 링크: "[[Creational Pattern]]"
---
# Prototype Pattern 
## Purpose
> Create objects based upon a template of existing objects through cloning

- 복제를 통해 이미 존재하는 객체를 복사하여 객체를 생성한다.
- 다른 생성 타입과 다른 점은, Prototype은 new 키워드와는 관련이 없다는 것이다.

## Use When
- 객체의 구성, 생성 및 표현은 시스템과 분리되어야 할 때
- 생성할 클래스는 런타임에 지정되어야 할 때
- 기존 객체나 객체 구조와 동일하거나 매우 유사한 객체 또는 객체 구조가 필요할 때
- 각 객체의 초기 생성이 비용이 많이 드는 작업일 때
- 제품의 클래스 계층구조와 병렬적인 공장 클래스 계층구조를 만드는 것을 피해야 할 때

## Participants
- **Prototype**
    - 자기 자신을 복제하는 연산에 대한 인터페이스를 정의한다.
- **ConcreteProduct**
    - 프로토타입을 구현하여 복제 연산을 제공하는 구체 클래스
- **Client**
    - 여러 오브젝트들을 복제하고자 하는 클라이언트

## Class Diagram
![](https://i.imgur.com/PqaX5ad.png)

## Sequence Diagram
![](https://i.imgur.com/N1K5u50.png)

## Cloning in Java
- Java는 **Cloneable 인터페이스**와 Object 클래스의 **protected clone 메서드**를 통해 클로닝을 지원한다.
    - 이는 기본적으로 메모리 카피를 통해 복제한다.
- 내장된 클로닝 메커니즘을 사용하는 클래스는 다음을 수행해야 한다:
    - **Cloneable 인터페이스**를 구현한다.
    - 구체적인 **public** 또는 **protected clone()** 메서드를 정의한다.
    - **clone() 메서드**에서 `super.clone()`을 호출하여 새 객체를 생성한다.
- 기본 clone() 메서드는 **얕은 복사(shallow copy)** 를 수행한다!
    - deep copy를 하려면 다른 작업을 추가로 수행해야한다.
- 이 약속은 나 뿐만 아니라 다른사람도 지켜야 효과가 나타난다.
    - 상위 어딘가에서 super.clone()을 안해준다면, object.clone까지 가지 않음.