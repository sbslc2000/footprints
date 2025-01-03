---
상위 개념: "[[../IPC]]"
---
# Shared Memory 
프로세스간에 공유할 수 있는 메모리 공간을 놓는 기법이다.
Shared Memory 모델에서는 협력 프로세스들에 의해 공유되는 메모리 영역이 구축된다. 프로세스들은 그 영역에 데이터를 읽고 씀으로써 데이터를 교환할 수 있다.

프로세스 메모리 공간에는 공유 메모리 영역을 저장할 수 있는 공간이 있다. 한 프로세스가 해당 영역에 메모리 영역을 구축하면, 다른 프로세스들은 해당 세그먼트를 자신의 주소 공간에 추가함으로써 접근할 수 있다. 일반적으로 서로 다른 프로세스의 메모리 영역에 접근하는 것은 운영체제에 의하여 금지되어 있기 때문에, 이 매커니즘이 동작하기 위해서는 두 프로세스가 이 제약 조건을 제거하는 것에 동의를 해야한다. 이렇게 연결이 되고 나면 이후 발생하는 데이터 교환은 더 이상 운영체제의 통제를 받지 않으며, 단순히 메모리 접근을 통해 데이터를 주고 받을 수 있다.

둘 이상의 프로세스가 하나의 자원 (메모리 공간)을 사용하는 것이기 때문에 동기화 이슈가 발생할 수 있으며, 메모리에 접근하고 조작하는 코드가 응용 프로그래머에 의해서 명시적으로 작성되어야 한다는 점에서 고려할 점이 많다.