---
상위 개념: "[[../Java|Java]]"
---
# CompletableFuture
CompletableFuture은 자바 8버전부터 도입된 비동기 작업 처리용 클래스이다. 스레드를 직접 관리하지 않고 코드를 깔끔하게 작성하게 도와준다.

```java
CompletableFuture<String> future = 
	CompletableFuture.supplyAsync(() -> "Hello, Completable Future!");

//callback
future.thenAccept(System.out::println);
```

별도의 start() 호출 없이 supplyAsync를 호출하면 비동기로 작업 함수가 수행된다.

## supplyAsync와 runAsync
`supplyAsync()`는 결과를 반환하는 비동기 작업을 시작한다. 반면 `runAsync()`는 결과를 반환하지 않는 비동기 작업을 시작한다.
```java
//runAsync는 결과 반환 x
CompletableFuture<Void> future =  
        CompletableFuture.runAsync(() -> System.out.println("Running in a separate thread"));  
```

## chaining 지원
`thenApply()`, `thenAccept()`, `thenCompose()`, `thenCombine()` 과 같은 메서드들을 통해 비동기 결과물을 체이닝 방식으로 조작할 수 있다.
```java
CompletableFuture  
        .supplyAsync(() -> "Hello")  
        .thenApply(s -> s + " World")  
        .thenApply(s -> s + "!")  
        .thenAccept(System.out::println);
```