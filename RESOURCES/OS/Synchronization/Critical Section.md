---
상위 링크: "[[Operating System]]"
---
# Critical Section
임계구역*Critical Section*이란 둘 이상의 스레드가 동시에 접근할 때에 문제가 발생할 수 있는 코드 영역을 의미합니다.
![](https://i.imgur.com/QW0Whux.png)

임계 구역에 둘 이상의 스레드가 동시 접근하여 공유 자원을 변경하는 경우 Race Condition이 발생하고 데이터의 일관성이 깨질 수 있습니다.