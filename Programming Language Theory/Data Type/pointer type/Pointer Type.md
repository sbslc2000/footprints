---
상위 링크: "[[Data Type]]"
---
# Pointer and Reference Types
포인터 타입이란 실제 데이터를 저장하는 타입과 다르게, 메모리 주소와 NIL을 가질 수 있는 데이터 타입을 의미한다.  포인터 타입은 간접 주소 지정 (indirect addressing) 과 동적 할당 관리 (dynamic storage management) 를 위해 사용하며, 언어의 작성력을 높여주는 효과를 갖는다.

>[!info]
>NIL이란 유효하지 않은 주소, 혹은 참조할 수 없는 주소를 가리키는 특수한 값이다.

## 설계 이슈
- 포인터 변수의 scope와 생애주기는?
- 동적 변수의 생애주기는?
- 포인터가 가리킬 수 있는 개체를 제한해야하나?
    - 이는 타입 검사와 매우 연관되어있다.

## Pointer Operations
포인터는 기본적으로 2가지 연산을 제공한다.
- 배정 연산(Assignment)
배정 연산은 포인터 변수에 특정한 개체의 주소를 할당하는 동작을 수행한다. 동적 할당은 암시적으로 포인터를 줄 수 있지만 간접 주소 지정은 명시적 연산자나 함수를 호출하는 방식으로 이용해야한다.
    
    ```c
    int * aa, bb;
    aa = &bb;
    ```

- 역참조(Dereferencing)
포인터가 가리키고 있는 주소의 값을 가져오는 연산이다.
```c
    int cc;
    cc = *aa;
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
