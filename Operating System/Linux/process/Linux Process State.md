---
상위 개념: "[[Linux Process]]"
---
# Process State
![](https://i.imgur.com/qXVzgQd.png)
프로세스 디스크립터의 state 필드는 프로세스의 현재 상태를 나타낸다. 시스템의 각 프로세스는 정확히 다섯 가지 상태 중 하나에 속한다.

### TASK_RUNNING
프로세스가 현재 실행 중이거나, 실행 대기 큐(run-queue)에 올라 실행을 기다리고 있는 상태를 의미한다.

### TASK_INTERRUPTIBLE
프로세스가 어떤 조건을 기다리며 잠들어있는 상태이다. 조건이 충족되면 커널은 프로세스의 상태를 TASK_RUNNING으로 바꾼다. 또한 시그널을 수신하면 조건 충족과 관계없이 미리 깨어나 실행 가능한 상태가 된다.

### TASK_UNINTERRUPTIBLE
이 상태는 TASK_INTERRUPTIBLE과 동일하지만, 시그널을 수신하더라도 깨어나거나 실행 가능 상태가 되지 않는다. 이는 프로세스가 반드시 중단 없이 기다려야 하는 상황이나, 이벤트가 곧바로 발생할 것으로 예상되는 상황에서 사용한다. 이 상태에서는 시그널에 반응하지 않기 때문에 TASK_UNINTERRUPTIBLE은 TASK_INTERRUPTIBLE보다 덜 자주 사용된다.

### TASK_TRACED
프로세스가 ptrace를 통해 디버거 같은 다른 프로세스에 의해 추적되고 있는 상태이다.

### TASK_STOPPED
프로세스 실행이 중단된 상태이다. 실행 중이지도 않고, 실행 가능 상태에도 있지 않다. 이는 태스크가 SIGSTOP, SIGTSTP, SIGTTIN, SIGTTOU 시그널을 수신했거나, 디버깅되는 도중 어떤 시그널을 수신했을 때 발생한다.