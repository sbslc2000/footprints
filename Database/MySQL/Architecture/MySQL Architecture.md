---
상위 개념: "[[MySQL]]"
---
# MySQL Architecture
MySQL Architecture는 MySQL 서버를 구성하고 있는 구성요소들을 의미한다.

MySQL Architecutre는 크게 [[MySQL Engine]]과 [[MySQL Storage Engine/MySQL Storage Engine]]으로 나눌 수 있다. 이들 사이에는 Handler API가 있고 스토리지 엔진은 이 API를 구현하여 MySQL Engine이 스토리지 엔진에 의존하지 않고 협업할 수 있도록 돕는다.
![](https://i.imgur.com/MufSN5D.png)

## MySQL 스레딩 구조
