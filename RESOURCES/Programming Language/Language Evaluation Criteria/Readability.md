---
상위 링크: "[[언어 평가 기준]]"
---
# 가독성 Readability

- 해당 프로그래밍 언어로 작성한 코드를 얼마나 쉽게 이해할 수 있는지 _ease with which programs can be read and understood._
- 과거에는 컴퓨터가 얼마나 효율적으로 명령을 실행할 수 있는지가 더 중요했다.
- 하지만 70년대 software life-cycle concept이 정립되면서, 유지보수가 소프트웨어에서 중요한 요소로 자리잡게 되었다.
- 가독성은 유지보수가 얼마나 쉬운지를 결정하기 때문에, 가독성은 프로그래밍 언어의 가장 중요한 평가기준이 되었다.
- **machine orientation to human orientation**
- Problem Domain
    - 용도에 알맞지 않은 언어를 사용한다면, 코드는 자연스럽지 않게 되고, 가독성은 떨어진다.

## 전반적인 단순성 _Overall Simplicity_

- 언어에서 지원하는 기본적인 구조가 많을수록, 배우기 어렵다 _a small number of elementary components are easy to read_
- _**Feature Multipliciy**_
    - 같은 기능을 수행하는데에 표현하는 방법이 여러가지라면 가독성에 매우 안좋다. _having more than one way to accomplish a particular operation_
    - Ex. `count++`, `++count`, `count += 1`
- _**Operator overloading**_
    - 하나의 연산자가 두 가지 이상의 의미를 갖는 것 _a single operator symbol has more than one meaning_
    - 가급적이면 하나의 연산자가 하나의 의미를 갖는 것이 좋긴하다.
- **그러나 너무 단순하다면 그것 역시 가독성에 좋지 않다.**
    - 너무 단순하다면 표현할 수 있는 한계가 있기 때문 _lack more complex control statements, program structure is less obvious_
    - 같은 operation에 대해 너무 긴 코드가 만들어진다.
    - Ex. Assembly

## 집교성 Orthogonality

- 결합할 수 있는 가지 수가 얼마나 많은가 _a relatively **small set of primitive constructs** that can be **combined in a relatively small number of ways** to build program_
- 집교성이 높다면, 언어의 규칙에 예외가 적다. _A lack of orthogonality leads to exceptions to the rules of the language._
- Ex. 나쁜 집교성 예시
    - int 배열에 대해서 ,,,
    - a[0]이 1000이면, a[1] 은 1004 이어야 한다.
    - *a가 1000이지만, *(a+1)은 1004이다.
    - 컴파일러는 a가 포인터인지 판단하여, + 1 이라는 연산에 대해 4를 곱하여 연산을 수행할지를 결정한다. → 포인터형 변수가 온다면, 그냥 더하는 것이 아닌 바이트 수만큼을 곱하는 예외가 발생
    
    ```jsx
    #include <stdio.h>
    int main() {
    	int * a = 10;
    	int b = 10;
    	
    	printf("%d", a + b); // 50
    }
    ```
    

## 적절한 제어문 _Control Statements_

- 코드는 일반적으로 위에서 아래로 읽는 구조여야 가독성이 올라간다.
- 코드를 읽는 도중 인접하지 않은 문장으로 점프하는 일이 잦다면 가독성은 낮다. _programs that require the reader to visually jump from one statement to some nonadjacent statement in order to follow the execution order_
- **GOTO-less**
    - goto는 ‘몇 번째 라인으로 가라’라는 의미를 가진 제어문이며, 이는 아래로 갈 수도 있고, 위로 갈 수도 있고, , 코드를 읽는 상황에서 시점과 문맥을 이동해야 한다는 점에서 가독성을 해친다.

## 데이터 타입과 자료구조 _Data Types and Structures_

- 충분히 유용할 정도로 데이터 타입과 구조를 제공해야 가독성이 높아진다. _adequate facilities for defining data types and data structures_
    - Ex. c언어는 boolean 타입을 지원하지 않는다. 정수형이 boolean 처럼 사용된다는 점에서 가독성을 해칠 수 있다.
    - Ex. Record, boolean type, enumeration type

## 구문 설계 _Syntax Considerations_

- _**Identifier forms**_
    - underscore를 지원하는가
- _**Special words**_
    - Ex. end_if, end_loop, …)
    - if () { … } vs if () … end if
- _**Form and meaning**_
    - `GO TO (10, 20, 30), l` ⇒ l이 0보다 작고 큼에 따라 10, 20, 30으로 이동해라
    - `GO TO l, (10, 20, 30)` ⇒ 얘는 또 다른 기능 → 10 , 20, 30에 포함된다면 이동해라
    - 한 가지 키워드가 여러 기능을 수행한다면 설계에서 바람직하지 않다.