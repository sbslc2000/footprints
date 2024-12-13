`fork()` 는 UNIX 시스템에서 부모 프로세스의 프로세스 주소 공간을 복사하여 자식 프로세스를 만드는 System Call이다. 원본 프로세스와 자식 프로세스는 fork() 후의 명령어부터 실행을 계속하며, fork()의 반환값은 부모 프로세스와 자식 프로세스에서 다르다. 부모 프로세스에서의 fork()는 반환값이 자식 프로세스의 PID이며, 자식 프로세스에서의 반환값은 0이다.

# 예시
```c
//fork.c

#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <unistd.h>

int main() {
	pid_t pid;
	int result; //자식 프로세스의 결과를 받을 변수
	pid = fork();

	if(pid < 0) {
		fprintf(stderr, "Fork Failed");
		return 1;
	} else if (pid == 0) { //자식 프로세스에서 조건 만족
		execlp("/Users/goorm/project/os/helloFork",NULL,NULL);
	} else { //부모 프로세스에서 조건 만족
		wait(&result); //자식 프로세스의 결과 상태값을 result에 저장
		if(result == 0)
			printf("Child Complete");
		else
			printf("Child Error");
	}
	return 0;
}
```

```c
//helloFork.c

#include <stdio.h>

int main(void) {
	printf("Hello, fork()!\n");
	return 0;
}
```

```bash
gcc helloFork.c -o helloFork
gcc fork.c -o fork
./fork.out


//Result
Hello, fork()!
Child Complete //이 값은 helloFork.c 의 main 함수의 반환값에 따라 달라짐
```