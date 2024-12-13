System call이란 User Mode에서 동작하는 프로세스 혹은 쓰레드가 운영체제의 핵심 기능을 사용하기 위해 요청하는 함수입니다. open(), write(), [fork()](fork()), exit() 등의 함수가 그 예이며, 프로세스가 system call을 호출하는 경우 Kernel Mode로 변환되며 운영체제가 요청받은 내용을 수행합니다.

