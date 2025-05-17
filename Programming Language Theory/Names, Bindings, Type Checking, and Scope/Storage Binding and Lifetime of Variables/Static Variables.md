---
상위 개념: "[[../Names, Bindings, Type Checking, and Scope|Names, Bindings, Type Checking, and Scope]]"
---
# Static Variables
정적 변수(Static Variables)란 실행 전에 메모리에 바인딩이 되고, 프로그램이 종료되기 전까지 같은 메모리 장소에 유지되는 변수를 말한다. c언어의 전역변수가 대표적인 정적 변수이며, 이들은 과거의 내역이 기록되기 때문에 history-sensible varialbles라고도 부른다.

## 특징
* 변수의 값에 대한 참조는 직접 참조로 이루어진다.
* 해당 메모리 공간은 런타임에 다른 변수가 사용하지 못한다.