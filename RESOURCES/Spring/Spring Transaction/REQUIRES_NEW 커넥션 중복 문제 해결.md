---
상위 개념: "[[트랜잭션 전파]]"
---
# REQUIRES_NEW DB 커넥션 중복 문제 해결

REQUIRES_NEW 트랜잭션 전파 정책을 사용하면 외부 트랜잭션에 대해 잠시 보류하고 새로운 트랜잭션을 생성하여 수행한다. 이 과정에서 하나의 쓰레드에서 2개 혹은 그 이상의 DB 커넥션을 점유할 수 있다.
![](https://i.imgur.com/VoczfKa.png)

이러한 문제를 해결하기 위해 facade 전략을 사용할 수 있다.

![](https://i.imgur.com/rXwCqxR.png)
위와 같이 분리되어야할 트랜잭션에 대해 각각 REQUIRE 정책을 사용하는 상태로 동기적으로 호출하면 쓰레드가 최초 트랜잭션 종료 이후 새로운 트랜잭션을 생성하므로 DB 커넥션을 중복으로 보유하지 않게 된다.ㄹㄹ