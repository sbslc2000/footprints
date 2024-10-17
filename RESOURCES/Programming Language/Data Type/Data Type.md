---
상위 링크: "[[RESOURCES/Programming Language/Programming Language|Programming Language]]"
---
> Computer programs는 데이터를 조작하여 결과를 생성한다. 이 작업을 쉽게 수행할 수 있는지 판단할 수 있는 중요한 요소는 _**데이터 타입이 실제 세계와 얼마나 유사한지**_ 이다. _how well the data types matches the real-world problem space_

# Data Type
- _**Data Type**_
    - 데이터 값들의 모임 _collection of data values_
    - 값들에 대한 미리 정의된 연산들의 집합 _predefined operations_
- 사용하는 언어에서 제공하는 데이터 타입이 **현실의 용어와 잘 매칭되는 것이 중요하다.** _the data types match the real-world problem space ⇒ easy of programming_
    - FORTRAN 90 이전에는 연결리스트와 트리가 전부 배열로 구현되었다.
    - COBOL은 10진수와 레코드에 대한 데이터 타입을 지원했다.
    - PL/I 는 범용성을 위해 많은 데이터 타입을 제공했다.
    - **ALGOL-68은 사용자 정의 타입을 지원했다.**
    - Ada 는 추상 데이터 타입 _Abstract Data Type_ 을 지원했다.
        - _**ADT**_
            - 직접적으로 구현방법을 명시하지 않고, 데이터와 데이터에 대한 연산을 나타내는 표현 방식 _**interface of a type**, which is visible to the user, is separated from the representation and **set of operations** on values of that type, which are hidden from the user._
        - 이는 11장에서 더 자세히 배울 것이다.