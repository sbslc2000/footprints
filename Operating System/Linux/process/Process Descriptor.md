---
상위 개념: "[[Linux Process]]"
---
# Process Descriptor
리눅스 커널은 프로세스 목록을 태스크 리스트(task list)라 불리는 원형 이중 연결 리스트에 저장한다. 태스크 리스트의 각 요소는 task_struct 타입의 프로세스 디스크립터이며, 이는 \<Linux/sched.h>에 정의되어 있다. 프로세스 디스크립터는 특정 프로세스에 대한 모든 정보를 담고 있다.

![](https://i.imgur.com/45Y0smN.png)

task_struct는 큰 자료구조로, 32비트 기계에서 약 1.7 킬로바이트 정도의 크기를 가진다. 이 자료구조에는 사용중인 파일, 프로세스의 주소 공간, 보류 중인 시그널, 프로세스의 상태 등의 정보가 포함된다.

## Allocating the Process Descriptor
커널 2.6 버전 이전에는 task_struct가 각 프로세스의 커널 스택 끝에 저장되었다. 이 구조에서는 x86과 같이 레지스터 수가 적은 아키텍처에서, 프로세스 디스크립터의 위치를 별도의 레지스터에 보관하지 않고도 스택 포인터를 통해 계산할 수 있었다.

하지만 2.6 버전 이후에는 프로세스 디스크립터가 슬랩 할당자(slab allocator)를 통해 동적으로 생성되면서, 새로운 구조체인 thread_info가 만들어졌다. 이 구조체는 스택이 아래로 자라는 경우 스택의 맨 아래에, 스택이 위로 자라는 경우 스택의 맨 위에 위치하게 된다.

![](https://i.imgur.com/1xfBkvB.png)

thread_info 구조체는 x86d에서 \<asm/thread_info.h>에 정의되어 있다.

```c
struct thread_info {
	struct task_struct *task; //task_struct를 가리키는 포인터
	struct exec_domain *exec_domain;
	__u32 flags;
	__u32 status;
	__u32 cpu;
	int preempt_count;
	mm_segment_t addr_limit;
	struct restart_block restart_block;
	void *sysenter_return;
	int uaccess_err;
};
```

## Storing the Process Descriptor
시스템은 프로세스를 고유한 프로세스 식별값(PID)로 구분한다. PID는 불투명 타입(Opaque Type) pid_t로 표현되는 숫자값이며, 일반적으로는 int 타입이다. 초기 유닉스 및 리눅스 버전과의 하위 호환성 때문에 기본 최대값은 단지 32,768이지만, 옵션으로 최대 400만까지 늘릴 수 있고 이는 \<linux/threads.h>에서 제어된다. 

> [!info] 불투명 타입
> 불투명 타입(Opaque Type)이란 구조체나 타입의 내부 구현을 외부에서 볼 수 없게 숨겨놓은 타입을 의미한다.

이 최대값은 시스템에서 동시에 존재할 수 있는 프로세스의 최대 개수를 결정한다. 또한 최대값이 낮을 수록 값이 빨리 순환(wrap around)하여, PID가 클수록 더 나중에 실행된 프로세스를 의미한다는 유용한 개념이 깨질 수 있다.

커널 내부에서 태스크는 일반적으로 task_struct 구조체를 가리키는 포인터로 직접 참조된다. 실제로 프로세스를 다루는 대부분의 커널 코드는 task_struct를 직접 사용한다. 따라서 현재 실행중인 태스크의 프로세스 디스크립터를 빠르게 찾아오는 기능이 필요하며, 이는 current 매크로를 통해 이루어진다. 이 매크로는 아키텍처 별로 독립적으로 구현되어 있다.

