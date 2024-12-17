---
상위 링크: "[[Creational Pattern]]"
---
# Builder Pattern

## Purpose
> Allows for the dynamic creation of objects based upon easily interchangeable algorithms

빌더 패턴은 쉽게 교체 가능한 알고리즘을 기반으로 객체를 동적으로 생성할 수 있게 해준다.

## Use When
* 런타임에 생성 프로세스를 제어하고 싶을 때
* 생성 알고리즘의 여러가지 표현이 필요할 때
* 객체 생성 알고리즘을 시스템과 분리하고 싶을 때
* 핵심 코드를 변경하지 않고 새로운 생성 기능을 추가해야할 때

## Participants
- **Client**
    - 프로덕트를 생성하기 위해 디렉터와 빌더를 선택한다.
    - 빌더에게 완성된 프로덕트*final constructed product*를 달라고 요청한다.
- **Director**
    - 프로덕트를 생성하기 위해 어떤 단계를 거쳐야하는지를 알고 있다.
    - **그러나, 디렉터는 각각의 단계를 어떻게 수행해야하는지는 모른다.**
- **Builder** (Abstract Class)
    - 프로덕트 객체를 만들기 위한 부품을 생성하는 인터페이스를 명시한다.
- **Concrete Builder**
    - 빌더 인터페이스를 구현함을 통해, 부품을 생성하고 이를 프로덕트에 조립한다.
    - 생성하는 표현의 구조를 정의하고 그 상태를 관리한다 _defines and keeps track of the representation it creates_
    - 프로덕트를 반환하는 인터페이스를 제공한다.
- **Product**
    - 만들고자 하는 복잡한 객체

## Class Diagram
![](https://i.imgur.com/VhkTaEP.png)

## Sequence Diagram
![](https://i.imgur.com/5z4lqPc.png)

## When a Builder Shouldn't Be Used
인터페이스가 안정적이지 않으면 Builder 패턴의 이점이 거의 없다. 인터페이스가 변경될 때마다 컨트롤러를 수정해야 하고 추상 기본 클래스나 그 객체들에 영향을 미친다. 새로운 메서드가 추가되면 기본 클래스와 이를 오버라이드해야 하는 모든 구체 클래스도 변경해야 한다. 또한 특정 메서드의 인터페이스가 변경되면 기존 메서드를 지원하던 모든 구체 클래스도 함께 수정해야 한다.

## Example Source Code
[design-pattern-example/src/main/java/io/github/sbslc2000/builder at main · sbslc2000/design-pattern-example · GitHub](https://github.com/sbslc2000/design-pattern-example/tree/main/src/main/java/io/github/sbslc2000/builder)