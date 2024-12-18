---
상위 개념: "[[Process 생명주기와 State]]"
---
# 5-Model Process State Diagram
![](https://i.imgur.com/My4oKiB.png)

프로세스를 효과적으로 관리하기 위하여 운영체제는 프로세스에 상태를 부여하고, 이를 PCB에 저장하여 활용합니다.

**Start 혹은 New 상태**는 프로세스의 생성을 요청받아 필요한 초기화를 하는 상태를 의미합니다. 이후 수행되기 위한 모든 준비가 완료되면 프로세스는 Ready 상태로 바뀝니다.

**Ready 상태**의 프로세스들은 CPU를 획득하기 위해 기다리고 있는 프로세스 상태를 의미합니다. 레디 상태에 있는 프로세스들의 PCB는 큐로 관리되어, context switch에 의해서 프로세스를 점유하게 될 시점을 기다립니다. 이후 context switch에 의하여 프로세스를 점유하게 되면, 프로세스는 Running 상태가 됩니다. 이때 context switch를 하는 과정을 dispatch라고 부르기도 합니다.

**Running 상태**의 프로세스는 현재 CPU에 의해 수행되고 있는 프로세스를 의미합니다. 만약 프로세서가 하나밖에 없다면 모든 프로세스 중 Running 상태의 프로세스 역시 하나밖에 없을 것입니다. Running 상태의 프로세스는 3가지의 상태로 바뀔 수 있습니다.
첫번째는 Ready 상태로 돌아가는 경우입니다. 이 경우는 프로세스가 CPU에 의해 수행되던 도중 동시성을 위해 context switch가 일어나는 경우입니다. 프로세스는 Ready 상태로 돌아가 자기 차례를 기다리게 됩니다.

두번째는 **Waiting 상태**로 바뀌는 경우입니다. 이 경우는 I/O Operation이 발생하여 Disk에서 필요한 값을 읽어올 때까지 기다리는 경우와 Time Slicing Interrupt가 발생하여 CPU를 내주는 경우를 포함합니다. 전자의 경우 프로세스는 I/O Operation이 끝나기 전까지는 프로세스를 점유할 필요가 없으므로 Ready가 아닌 Waiting 상태로 바뀝니다. 이후 I/O Operation이 끝나고 Interrupt가 발생하면 OS는 Waiting 상태를 Ready 상태로 바꾼 뒤 Ready Queue에 PCB를 담습니다. 이후 CPU를 획득한다면 I/O 결과물을 가지고 연산을 이어갈 수 있습니다.

세번째는 **Termination 상태**로 바뀌는 경우입니다. 이 경우는 프로그램의 모든 명령어를 수행하고 종료되는 상황입니다. Termination 상태가 된 프로세스는 운영체제에 의해 사용했던 모든 자원을 반납하고 종료됩니다. 또한 운영체제는 예기치 못한 상황이 발생하여 프로세스를 종료해야할 때 Termination 상태로 바꾸기도 합니다.