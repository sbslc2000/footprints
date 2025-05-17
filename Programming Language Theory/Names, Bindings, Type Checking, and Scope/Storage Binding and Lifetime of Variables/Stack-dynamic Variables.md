---
상위 개념: "[[../Names, Bindings, Type Checking, and Scope|Names, Bindings, Type Checking, and Scope]]"
---
# Stack-dynamic Variables
스택 동적 변수(Stack-dynamic Variables)란 메모리의 스택 영역에 저장되는 변수들로, c언어 기준 지역 변수, 인수, 반환 값 등이 있다. 스택 동적 변수의 타입은 컴파일 타임에 바인딩되며, 값은 런타임 도중 해당 선언문에 도달했을 때 메모리에 바인딩된다.

## 특징
* 재귀를 구현할 수 있는 기반이 된다.
* 서로 다른 프로시져에서 동일한 공간을 차지하게 만들 수 있어 메모리 효율성이 높다.
* 메모리 할당 및 해제가 런타임에 수행되므로 런타임 오버헤드가 발생한다.
* 통상적으로 간접주소(indirect-addressing)를 사용하기 때문에 접근 속도가 정적 변수에 비하여 느리다.