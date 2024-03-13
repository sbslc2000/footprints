---
상위 링크: "[[GRASP]]"
---
# Creator Pattern
Creator Pattern이란 A라는 객체가 만들어질 필요가 있을 때 이것을 **누가 만들지**에 대한 답을 주는 패턴이다.

## Solution
Createor 패턴은 다음과 같은 특징을 갖는 클래스에 생성 책임을 부여할 것을 권장한다.

* **A라는 객체를 contain하고 있거나 aggregate 관계에 있을 때**
* A를 recording하고 있을 때
* A를 밀접하게 사용할 때
* A를 생성하기 위한 데이터가 있을 때

또한 Creator 패턴은 위 조건이 여러개를 만족하는 경우, 첫번째 조건을 우선시할 것을 주장한다.

## Discussion
Creator Pattern은 생성 책임을 누구에게 할당할 것인가를 고민할 때 도움을 준다. 하지만 만약 instance 생성이 매우 복잡하고 중요한 경우 Creator Pattern과는 다른 방법을 사용하는 것이 좋을 수도 있다.

* 성능 이유로 인스턴스를 재사용해야 할 때 -> 싱글톤 패턴을 쓰는 것이 좋을 수도
* 여러 유사한 클래스로부터 만들어질 수 있는 경우 -> 주로 Abstract Factory에 책임을 할당
* 쉽게 갈아낄 수 있어야 하는 경우 -> DI Container

## Benefits
Creator Pattern은 객체간의 Coupling을 낮추는데 도움이 된다.


