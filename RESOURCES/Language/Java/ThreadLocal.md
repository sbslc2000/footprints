---
상위 개념: "[[Java]]"
---
# ThreadLocal
ThreadLocal은 멀티쓰레드 환경에서 쓰레드 영역에 변수를 저장할 수 있게 해주는 기능이다.

```java
ThreadLocal<String> str = new ThreadLocal<>(); str.set("Hello, ThreadLocal!"); System.out.println(str.get()); //Hello, ThreadLocal! 
str.remove();
```

Thread Pool을 사용할 때에, 과거에 사용됐던 쓰레드 사용자가 저장했던 변수를 지워주지 않으면, 이후 쓰레드를 획득한 측에서는 그 정보를 그대로 볼 수 있다. 따라서 항상 저장공간을 다 사용한 후에는 remove() 메서드를 호출해야 한다.