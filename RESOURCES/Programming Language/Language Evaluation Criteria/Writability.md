---
상위 링크: "[[언어 평가 기준]]"
---

# 작성력 Writability

- 풀고자하는 문제가 있을 때, 프로그래밍 언어를 얼마나 쉽게 사용하여 문제를 해결할 수 있는가를 의미한다. _how easily a language can be used to create programs for a chosen problem domain_

### 간단함과 직교성 _Simplicity and Orthogonality_

- 언어의 규칙이 적고 발생할 수 있는 조합이 적다면, 작성력이 높다. _a small number of primitive constructs and a consistent rules for combining them (that is, orthogonality) is much better than simply having a large number of primitives_

### 추상화에 대한 지원 _Support for Abstraction_

- Abstraction : _the ability to define and then use complicated structures or operations in ways that allow many of the details to be ignored_
    - _**Process abstraction**_ : 프로세스 추상화
        - 동작을 추상화 하는 것 → subprogram (함수)
    - _**Data abstraction**_ : 데이터 추상화
        - 데이터 구조를 추상화 하는 것 → Record Type (마치 구조체와 같은)

### 표현력 _Expressivity_

- 같은 양의 연산을 보다 적은 코드로 수행할 수 있게 만드는 것 _a language has relatively convenient, rather than cumbersome, ways of specific computations_
    - Ex. `count++` 는 `count = count + 1` 보다 간단하고 짧다.