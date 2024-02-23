# Generic Programming
> Generic programming centers around the idea of abstracting from concrete, efficient algorithms to obtain generic algorithms that can be combined with different data representations to produce a wide variety of useful software.

구체적인 알고리즘(+ 로직, 행동)을 하나의 알고리즘(+ 로직,행동)으로 추상화하는 프로그래밍 방식

![](https://i.imgur.com/14mcrGu.png)

등록이라는 알고리즘은 동일한데 이를 처리해야할 다양한 타입이 있는 경우, register 로직을 추상화하고 함수에 들어올 수 있는 타입을 일반화하는 것

## 특징
![](https://i.imgur.com/xtWYIxv.png)

* 타입은 추후에 확정된다. *Type is specified later*
* 선언과 정의는 뭉뚱그려서 하지만, 사용될 때에는 타입을 정의하여 사용해야한다. *Instantiated with specific type*
* 사용되는 타입은 **컴파일타임에 결정**된다. *Determined in compile-time*
* 함수를 위한 template으로써 사용되며, 프로그램 내에 필요로 하는 타입 별로 함수가 여러개 만들어진다. 이렇게 분화되는 것을 specialization이라고 한다.
* 분화되는 시점이 컴파일 시점이므로 **run-time의 시간복잡도와는 상관이 없다.**
* 런타임에는 각각의 타입을 처리하는 함수만이 남고, [제너릭 타입과 관련된 정보는 사라진다.](Type%20Erasure) *Type Erasure*

## Code Explosion
제너릭 타입에 대응하는 파라미터 타입이 많을 경우 함수가 매우매우 많아져서 프로그램의 크기가 커지는 현상. 

## 다형성의 측면에서
> Polymorphism — providing a single interface to entities of different types. Virtual functions provide dynamic (run-time) polymorphism through an interface provided by a base class. Overloaded functions and templates provide static (compile-time) polymorphism

![](https://i.imgur.com/jQkeaNv.png)

다형성은 하나의 모습을 가진 메서드가 다양한 동작을 할 수 있게 하는 것으로, Run-time Polymorphism과 Compile-time Polymorphism으로 나뉠 수 있다.

Generic Programming은 Compile-time Polymorphism의 한 종류이다.