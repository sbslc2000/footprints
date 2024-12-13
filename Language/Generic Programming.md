# Generic Programming

> Generic programming centers around the idea of **abstracting from concrete, efficient algorithms** to obtain **generic algorithms** that can be combined with **different data representations** to produce a wide variety of useful software.

제너릭 프로그래밍이란 구체적인 알고리즘, 로직, 혹은 행동을 서로 다른 데이터 타입에 대해 적용하기 위해 하나의 일반화된 알고리즘, 로직, 혹은 행동으로 추상화하는 프로그래밍 방법입니다.

![](https://i.imgur.com/14mcrGu.png)

이는 추상화의 한 방법이기에, 소프트웨어 문제를 어떻게 추상화하고자 하는가에 따라 같은 코드라도 다른 방법이 채택될 수 있습니다. 만약 Student, Lecturer, Faculty라는 개념이 있고, 이들이 모두 등록될 수 있어야 한다면, 이를 객체들이 가져야할 일종의 계약으로 보고 구현을 강제하자는 측면에서는 인터페이스를 사용할 수 있을 것이며, 등록 로직은 동일하니 여러 타입을 받도록 하자는 측면에서는 제너릭 메서드를 사용할 수 있을 것입니다.

## 특징
![](https://i.imgur.com/xtWYIxv.png)

* 타입은 추후에 확정된다. *Type is specified later*
* 선언과 정의는 뭉뚱그려서 하지만, 사용될 때에는 타입을 정의하여 사용해야한다. *Instantiated with specific type*
* 사용되는 타입은 **컴파일타임에 결정**된다. *Determined in compile-time*
* 함수를 위한 template으로써 사용되며, 프로그램 내에 필요로 하는 타입 별로 함수가 여러개 만들어진다. 이렇게 분화되는 것을 specialization이라고 한다.
* 분화되는 시점이 컴파일 시점이므로 **run-time의 시간복잡도와는 상관이 없다.**
* 런타임에는 각각의 타입을 처리하는 함수만이 남고, [제너릭 타입과 관련된 정보는 사라진다.](Type%20Erasure) *Type Erasure*

## Compile-time Polymorphism vs Run-time Polymorphism

![](https://i.imgur.com/mgXJH73.png)


> _Polymorphism — providing a single interface to entities of different types. **Virtual functions** provide **dynamic (run-time) polymorphism** through an interface provided by a base class. **Overloaded functions** and **templates** provide **static (compile-time) polymorphism**_

다형성은 하나의 이름을 가진 메서드로부터 여러 동작이 수행될 수 있는 것을 의미합니다. 다형성은 두 갈래로 나뉩니다.

**Dynamic Polymorphism**, 혹은 Run-time Polymorphism으로 불리는 것으로 오버라이딩에 의해 하나의 메서드가 실제 참조 객체를 기반으로 하여 서로 다른 동작을 하는 것을 의미합니다. 이 때 동작이 결정되는 시점은 런타임이므로, 이러한 다형성은 런타임 다형성이라고 부릅니다.

한 편, 하나의 이름을 가진 메서드의 동작이 컴파일 타임에 서로 다른 동작으로 결정되는 경우도 있습니다. 오버로딩과 Generic 메서드가 그 예시입니다. 이러한 다형성은 **Static Polymorphism**, 혹은 Compile-time Polymorphism이라고 부릅니다.

오버로딩은 서로 다른 파라미터를 통해 하나의 이름을 가진 메서드가 서로 다른 명령을 수행하게 만드는 방법입니다. 제너릭은 일반화된 타입으로 다양한 인자를 처리할 수 있게 하는 것입니다. 이들은 모두 하나의 메서드 이름으로 여러가지 동작을 할 수 있게 만들면서, 어떤 동작을 수행할 지에 대해 컴파일 타임에 결정된다는 특징이 있습니다.