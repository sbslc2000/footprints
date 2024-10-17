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

### Dangling Pointer

- 허상 포인터로 번역됨
- 회수된 기억공간을 가리키는 포인터 _pointer that contains the address of dynamic variable that has been deallocated_
- 이미 해제된 이후에도 해당 공간을 가리키고 있으므로, 의도와 다르게 동작할 수 있다.

```c
int * 1;

sub1() {
	int j;
	j = 5;
	i = &j;
}

*i = ??
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
    

## Reference Types
    
- 포인터와 굉장히 유사
- **포인터는 메모리의 주소를 참조하지만, 레퍼런스 타입은 객체나 값을 참조한다.** _refers to an object or value in memory_
- Reference type은 C++에서 도입되었다.
    - **항상 암시적으로 역참조되는 상수 포인터** _constant pointer that is always implicitly dereferenced_
        - 레퍼런스 타입의 구현 자체는 포인터로 되어있다.
    - 정의할 때 항상 어떤 변수의 주소값으로 초기화가 된다.
    - 상수이기 때문에, 다른 object를 **가리키도록 바뀔 수 없다.** _can never be set to reference any other variable_
    
    ```c
    int result = 0;
    int &ref_result = result;
    
    ref_result = 100; //ref_result와 result는 alias이다.
    ```
    
    - **C++에서는 reference type을 통해 인수를 변경하여 함수 바깥의 값을 바꿀 수 있었다.**
        - c언어에서는 기본적으로 call by value이기 때문에, 포인터를 쓰지 않으면 인수를 변경할 수 없었다.
        - ⇒ 함수 호출에서의 **양방향 통신** _two-way communication_ 을 만들기 위해 사용

## Implementation
    
- 포인터의 구현 자체는 쉽다.
    - 포인터는 하나의 word 사이즈로 만들어진다. _pointers are single values stored in either two or four byte memory cell_
    - Ex. `int * a` 가 필요한 공간과 `double * a`가 필요한 공간이 같다.

### Solutions to the Dangling Pointer Problem

- _**Tombstone Approach**_
    - 동작 원리
        - Tombstone이라는 특수한 공간을 만든다. _tombstone is a special cell that have all dynamic variable_
        - 포인터는 동적 변수를 직접 가리키지 않고, Tombstone을 통해 동적 변수를 가리키게 한다.
        - 동적 할당이 해제되면, tombsone의 값은 NIL을 가리키게 한다. _the tombstone remains but is set to nil, it means dynamic variables no longer exists_
            - 이 경우, 해당 위치를 가리키던 다른 포인터 변수들은 자연스럽게 NIL을 가리키게 된다.
        - 장단점
            - tombstone의 공간은 프로그램에서 사용되는 동적 변수의 포인터 개수의 2배이다.
            - 참조하는데 시간이 더 걸린다.
        - 옛날의 맥킨토시, C++의 일부 라이브러리에서 이를 활용하고 있다.

- _**Locks-and-key Approach**_
    - 동작 원리
        - 동적 할당이 발생할 때, 포인터가 할당되는 메모리 공간에 key 값을, 동적할당되는 공간에는 lock 값을 넣어준다. _pointer values are represented as ordered pair of key-address_
            - heap 영역에는 lock 값이 헤더로 존재
            - pointer 영역에는 Key값이 헤더에 존재
            - 두 값은 동일하게 세팅된다.
            - 또 다른 포인터가 이를 가리킨다면 key 값에 lock값을 할당해주어야 한다.
        - 만약 도중에 해제되고 새로운 동적할당이 발생했는데 우연치않게 같은 공간에 할당된다면, **기존의 포인터의 key와 새롭게 할당된 공간의 lock 값이 다르므로 유효하지 않은 참조로 처리할 수 있다.**
            - 이를 위해서는 모든 접근마다 포인터의 key와 데이터의 lock을 비교해주어야 한다. _compares the key value of the pointer to the lock value in the dynamic variable_
        - 메모리 해제시 lock value도 초기화해야한다.
- **Heap Management**
    - Garbage를 어떻게 치울 것인가에 대한 질문은 힙 공간을 어떻게 관리해야하는가의 질문과 동일하다.
    - 힙 공간은 균등하게 할당되지 않으므로 힙 공간의 관리는 간단한 문제가 아니다.
    - 문제를 쉽게 풀기 위한 가정 : 모든 힙 공간은 고정된 크기로 할당된다.
    - 할당
        - 가용공간에 대한 리스트를 유지하며, 필요공간이 생길 때마다 해당 리스트로부터 요청한다.
    - 회수
        
        - 어떻게 쓰레기를 찾아야 하는가?
        - 한 개 이상의 포인터는 동적 공간을 가리킬 수 있다.
        - 포인터 하나가 해제되더라도, 더 이상 안쓰이는 공간인지 보장할 수 없다.
        
        1. _**Reference Counter Approach**_
            - **그때 그때 해제하는 방식** _incremental reclamation_
                - 접근할 수 없는 메모리 공간이 생길 때마다 회수한다.
            - 동적 변수 앞에 **counter**라는 공간을 만들어서, 현재 몇 개의 포인터가 가리키고 있는지 유지한다. _maintaining a counter in every cell, which stores the number of pointers that are currently pointing at cell_
            - **counter의 값이 0이 되는 연산이 발생한다면, 해당 공간을 회수한다.** _when reference counter reaches zero_
            - 구현 방법은 간단하지만 몇가지 문제점이 있다.
                - 셀의 크기가 작을 수록 counter의 크기는 공간적 부담이 된다. _space requirement for the counter_
                - counter의 값을 매번 변경시켜주어야 하는 실행시간이 부담이 된다. _execution time to main the counter_
                - Circular Reference, a가 b를 가리키고 b가 c를 가리키고, c가 a를 가리키는 경우 해당 공간은 영원히 해제되지 않는다. _circular reference_
        2. _**Garbage Collector**_
            - **한 번에 회수하는 방식** _batch reclamation_
            - 중간에 생성되는 쓰레기들은 신경쓰지 않는다. 포인터가 없어지면 garbage는 남아있다.
            - 더 이상 가용공간이 없어질 때까지, 회수를 진행하지 않는다.
            - 만약 할당이 필요한데 가용공간이 없다면, 쓰레기를 수집한다. _a garbage collection process is begun to gather all the garbage left floating around the heap_
            - 어떤 것이 쓰레기고 어떤 것이 아닌지 어떻게 구분하는가?
                - Garbage Collecting Algorithm이 있음
                    1. 힙의 모든 셀에대해 **쓰레기인지 아닌지 나타내는 표시** _indicator_ 를 한다(비트 하나를 넣는다.).
                    2. 모든 포인터를 통해 도달 가능한 셀은 쓰레기가 아니고, 도달 못하는 셀이 있다면 쓰레기가 아니다.
                    3. 이후 공간을 회수한다.
            - **When you need it most, it works the worst**
                - 조사하고 가용공간을 확보하는 시간이 매우 길다.
                - 비트를 만들고, tracing을 하고, 이 모든 작업은 할당을 하고자 할 때 발생한다.
            - **더 나아가, 셀의 크기는 모두 다르므로 추가적인 관리포인트가 생긴다.**
                - 셀이 garbage인지 체크하기 위한 비트를 어떻게 놔야하는지 판단하기 어렵다.
                    - 대응법 : 셀의 초반부에 시작지점과 끝지점을 기록해놓을 수 있다.
                - **힙 공간에 할당은 되는데, 꼭 포인터가 가리키지 않는 공간 역시 존재한다.**
                    - 대응법 : **가짜 포인터**를 만들어서 그것을 가리키게 만들 수 있다.
                - **파편화에 의한 문제**