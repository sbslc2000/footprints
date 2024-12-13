---
상위 링크: "[[Parameter Passing Methods]]"
---
# Pass-by-reference
Pass-by-reference는 In-out Mode에 해당하는 파라미터 전달 방식이다.

Pass-by-reference는 값을 물리적으로 복사하는 방식이 아닌, 접근 경로를 서브프로그램에게 제공하는 방식으로 동작한다. 접근 경로가 제공되기 때문에 서브프로그램에서 발생하는 값 변경이 실매개변수에 실시간으로 적용되는 효과를 낳는다. (공유, Alias)

```c
#include <stdio.h>
void swap(int a, int b) {  
    int temp = a;  
    a = b;         
    b = temp;     
}

int main() {
    int x = 10, y = 20;  // 초기 값 설정

    printf("Before swap: x = %d, y = %d\n", x, y); //10, 20

    swap(x, y);

    printf("After swap: x = %d, y = %d\n", x, y); //20, 10

    return 0;
}
```


## C언어에서 포인터를 전달하면 Pass-by-reference인가?

