---
상위 링크: "[[Object-oriented Programming]]"
---
# Inheritance

프로그래밍 언어를 이용해 일반화와 특수화의 관계를 구현하는 가장 일반적인 방법은 클래스 간의 상속을 사용하는 것이다. 하지만 안타깝게도 모든 상속 관계가 일반화 관계인 것은 아니다.

일반화의 원칙은 한 타입이 다른 타입의 서브타입이 되기 위해서는 슈퍼타입에 순응(conformance)해야 한다는 것이다. 순응에는 구조적인 순응과 행위적인 순응 두 가지가 있다. 두 가지 모두 특정 집합에 대해 대체 가능성을 의미하지만, 구조적 순응의 경우 속성과의 연관관계에 의한 대체 가능성에 관한 것이며, 행위적인 순응의 경우 동일한 계약을 기반으로 하냐와 관련이 있다.

## Subtyping과 Subclassing

서브클래스가 슈퍼클래스를 대체 할 수 있는 경우 이를 Subtyping이라고 하며, 서브클래스가 슈퍼클래스를 대체할 수 없는 경우를 Subclassing이라고 한다.

Subtyping의 목표는 설계의 유연성이 목표인 반면 Subclassing은 코드의 중복 제거와 재사용이 목적이다. 흔히 subtyping을 인터페이스 상속(interface inheritance)라고 하며, subclassing을 구현 상속(implementation inheritance)이라고도 한다.

어떤 한 클래스가 다른 클래스를 상속받았다는 사실만으로 두 클래스 관계가 서브타이핑인지, 서브클래싱인지의 여부를 결정할 수 없다. 이는 순전히 클라이언트 관점에서 확인해야하는 내용이다.