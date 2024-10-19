---
상위 링크: "[[Data Type]]"
---
# Pointer and Reference Types
- 포인터 타입
    - 실제로 데이터를 저장하는 다른 타입과 다르게, **메모리 주소**와 **NIL** 을 가질 수 있는 데이터 타입 _variables have a range of values that consists of memory address and a special value, **NIL**_
        - _**NIL**_ : 유효한 주소가 아니다. 참조할 수 없다. 를 가리키는 특수한 값 _nil is not a valid address, pointer cannot currently be used to reference a memory cell._
    - 포인터는 **간접 주소 지정** _indirect addressing_ 과 **동적 할당 관리** _dynamic storage management_을 위해 사용한다.
        - pointer는 heap을 가리킬 수 있기 때문에
- 포인터 타입을 프로그램의 writability를 높여준다.
    

## 설계 이슈

- 포인터 변수의 scope와 생애주기는?
- 동적 변수의 생애주기는?
- 포인터가 가리킬 수 있는 개체를 제한해야하나?
    - 이는 타입 검사와 매우 연관되어있다.

## Pointer Operations

- 포인터는 기본적으로 2가지 연산을 제공한다.
- _**Assignment :**_ **배정 연산**
    - 포인터 변수에 특정한 개체의 주소를 할당하는 것 ****_Set a pointer variable to the address of some object_
    - 동적 할당은 암시적으로 포인터를 줄 수 있지만, indirect addressing은 explicit operator나 build-in subprogram이 있어야한다.
    
    ```c
    int * aa, bb;
    aa = &bb;
    ```

- **_Dereferencing_ 역참조 ****
    - 포인터가 가리키고 있는 주소의 값을 가져오는 것

    ```c
    int cc;
    cc = *aa;
    ```
    
## Pointer and Pointer Problems in PL

### Type Checking

```c
*a > 10
```

- 부등호 연산을 하려면 숫자 타입이어야 비교가 가능하다.
- 하지만 변수 a가 가리키는 것이 수치 타입인 것을 어떻게 보장할 것인가?
    - 이것은 실행시간에야 알 수 있다
- **domain type**: 포인터가 가리킬 수 있는 타입
    - PL/1 의 포인터는 특정한 데이터 타입으로 한정되지 않았다. _not restricted to a single domain type_
    - C언어는 포인터 변수의 타입을 제한했다.
        - `int * a` 는 항상 int type만 가리킬 수 있다.

```

### Lost Objects

- 분실 객체
- 기억공간에 데이터는 저장되어있지만, 참조하고 있는 포인터가 없는 경우 _allocated dynamic object that is no longer accessible to the user program but may still contain useful data_
- 더 이상 목적대로 쓸 수 없으며, 이 공간은 재사용될 수도 없음
- 가비지 _garbage_ 라고 부르기도 함.

```c
char *c;
c = malloc(...); 여기서 XX0번지 공간 할당

c = malloc(...); 여기서 XX1번지 공간 할당, XX0번지는 접근할 수 없으면서 유지되고 있는 공간
```

## Pointers in Pascal
    
- 포인터는 동적 할당된 변수에 접근하는 용도로만 사용된다. _only to access dynamically allocated anonymous variable_
    - 할당에는 `new`
    - 해제에는 `dispose`
- Pascal은 교육용으로 설계된 언어므로 내제된 문제가 많은데, `dispose` 는 그 일례 중 하나이다.
    - **pascal의 dispose는 Dangling Pointer를 만든다.**
- 이에 대한 해결책
    - 컴파일러가 dispose를 무시하게 만든다.
    - dispose를 사용하지마세요
    - dangling pointer를 허용한다.
    - 모든 포인터를 검사해서, 동적 변수가 없어질 때 NIL을 가리키게 한다.
        - 해제 시 모든 포인터를 검사해야한다는 구현의 어려움 및 cost가 발생
        - 스택으로 구현할 수 있다! _mark and release the stack_

## Pointers in C

- assembly 언어와 비슷하게 주소를 가리키는 방식으로 사용할 수 있다.
- **C 언어의 포인터는 Dangling Pointer와 Lost Objects에 대한 어떠한 해결책도 지원하지 않는다.**
- **C 언어는 포인터에 대한 산술연산을 지원한다!** _Pointer arithmetic is possible_
    - “ptr + index”
    - 산술 연산은, 포인터 자료형의 크기만큼 증가시키고 감소된다. _scaled by the size of the object to which ptr is pointing_
    - Ex. `int * a = 1000; *a + 1 (1004)`


> [! info]
> 설계 문제
> 1. goto
> 	1. 제어의 범위를 확대시킴으로써 안좋은 효과
> 2. pointer
> 	1. _The introduction of pointer to high-level languages has been a step backward from which we may never recover_
> 	2. 메모리 셀의 사용 범위를 확대시킴
> 	3. 그 당시 설계에는 합리적 이유가 있었지만 (효율적인 메모리 관리), 메모리의 크기는 생각보다 빠르게 발전했다.
> 	4. C언어에서 이제는 하위호환 문제가 발생하여 함부로 없애지도 못한다.
    


## Implementation
    
- 포인터의 구현 자체는 쉽다.
    - 포인터는 하나의 word 사이즈로 만들어진다. _pointers are single values stored in either two or four byte memory cell_
    - Ex. `int * a` 가 필요한 공간과 `double * a`가 필요한 공간이 같다.
