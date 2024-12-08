---
상위 링크: "[[언어 평가 기준]]"
---
# Reliability

- 신뢰성이란, 만약 작성한 프로그램이 모든 조건하에 주어진 명세에 따라 동작하는 것을 의미한다. _if it performs its specification under all conditions_

## 타입 검사 _Type Checking_

- 타입 간 연산 혹은 오인을 검사할 수 있어야함 _testing for type compatibility between two variables or a variable and a constant that are somehow related with one another_
    - 컴파일 타임에 수행할 수도 있으며, 런타임에 수행할 수도 있다.
- 런타임 타입 체크는 오버헤드가 있으므로, 컴파일 타임에 하는 것이 더 좋다. _compile-time type checking is more desirable_
- C언어는 대표적으로 타입 검사를 하지 않는 언어이다. _no type checking for parameter passing_
    
    ```c
    void test(int a) {
    	printf("%d", a);
    }
    
    int main() { {
    	double f = 3.14;
    	test(f); 
    }
    
    // 3
    ```
    
- 반면 Java의 경우 타입 검사에 매우 엄격한 편이다.
- in Pascal: _the subscript range checking_ is part of type checking, although it must done at run time
    

### 예외 처리 _Exception Handling_
- 컴파일 타임에는 알 수 없고, 런타임에 발생할 수 있는 오류를 대처하기 위한 방안 _The ability of a program to intercept run-time errors, as well as other unusual conditions, to take corrective measures, and to continue is a great aid to reliability_
- C언어에서 예외처리는 필수가 아니다. → 신뢰성에 좋지 않다.
- Java의 경우 Exception을 처리해주지 않으면 컴파일 자체가 되지 않는다.

### 별칭 _Aliasing_
- Aliasing
    - 동일한 메모리 주소를 가리키는 이름이 2개 이상일 수 있는 특성 _having two distinct referencing methods, or names, for the same memory cell_
    - Ex. Equivalenced variable in Fortran, the pointers in Pascal
- 신뢰성에는 악영향을 미친다.
    - 왜냐? 의도치 않게 데이터를 변경할 수 있기 때문
- 그래서 대부분의 언어는 이런 사용을 지양하는데, 일부 언어는 제한적으로 aliasing을 수행하게 만든다. _It is now widely accepted that aliasing is a dangerous feature in programming language_

```c
// a와 b가 동일한 메모리 공간을 가리키고 있음! 신뢰성 하락!
int main () {
	int a = 10;
	int * b = &a;
}
```

### 가독성과 작성력_Readability and Writability_
- 가독성과 작성력이 낮다면, 유지보수에 어려움이 생길 뿐더러, 자연스럽지 않은 코드를 만들어낼 가능성이 있다. _Unnatural methods are less likely to be correct for all possible solutions_