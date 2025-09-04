# Linux Kernel


## Linux Kernel Development
리눅스 커널은 일반적인 사용자 공간 애플리케이션과 비교했을 때 몇 가지 고유한 특성을 가진다. 

* 커널은 C 라이브러리나 표준 C 헤더에 접근할 수 없다.
* 커널은 GNU C로 작성되어 있다.
* 커널은 사용자 공간에서 제공되는 메모리 보호 기능을 가지고 있지 않다.
* 커널은 부동소수점 연산을 쉽게 실행할 수 없다.
* 커널은 프로세스마다 고정된 크기의 작은 스택만을 가진다.
* 커널은 비동기 인터럽트를 가지고, 선점형이며, SMP를 지원하기 때문에 동기화와 동시성이 중요한 고려사항이다.
* 이식성이 중요하다.

### Inline Function
```c
static inline void wolf(unsigned long tail_size)
```
C99와 GNU C는 인라인 함수를 지원하며, 커널 소스코드는 인라인 함수를 적극적으로 사용한다. 인라인 함수란 함수의 명령들이 별도의 함수 스택을 생성하지 않으며, 호출자의 코드에 복사되는 함수를 의미한다. 이는 함수의 호출-반환 (레지스터 저장과 복구)에 의한 오버헤드를 줄여주지만, 코드의 바이너리 크기가 증가하고 캐시 사용량이 증가한다는 단점이 있다.

### Inline Assembly
```c
unsigned int low, high;

asm volatile("rdtsc" : "=a" (low), "=d" (high));

/* low and high now contain the lower and upper 32-bits of the 64-bit tsc */
```
gcc C 컴파일러는 일반적인 C 함수 안에 어셈블리 명령어를 삽입하는 것을 가능하게 한다. 이 기능은 특정 시스템 아키텍처에 고유한 커널의 일부에서만 사용된다.

