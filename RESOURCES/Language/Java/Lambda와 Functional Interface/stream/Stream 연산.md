---
상위 링크: "[[Stream]]"
---
# Stream 연산

## 중간 연산
### filter
filter는 주어진 요소를 Predicate 를 통과시켜 조건식이 참인 것들만 남긴다.
```java
IntStream.range(1, 10)
	.filter(i -> i % 2 == 0) // 2 4 6 8
```

### skip
skip은 앞에서 주어진 개수 만큼의 요소를 제거한다.
```java
IntStream.range(1, 10)
	.skip(5); // 6, 7, 8, 9
```

### map
map은 어떤 요소를 Function의 결과로 바꾼다. 이 과정을 통해 타입이 변경될 수 있다.
```java
IntStream.range(1, 10)
	.map(i -> i * 10); // 10, 20, 30, ...
```

### distinct
distinct는 중복 요소를 제거한다.
```java
IntStream.of(1,1,2,2,3)
	.distinct(); // 1, 2, 3
```

### sorted
sorted는 요소를 정렬한다.
```java
IntStream.of(3,2,5,1,4)
	.sorted(); /// 1,2,3,4,5
```

### boxed
boxed는 원시 타입을 래퍼 타입으로 변경한다.
```java
IntStream.of(3,2,5,1,4)
	.boxed() // Integer 3, 2, 5, 1, 4
```

### peek
peek은 중간 과정 중 스트림에 영향을 끼치지는 않으면서, Consumer 작업을 수행한다.
```java
IntStream.range(1,10)
	.peek(System.out::println) // 1 부터 10까지 출력 이후 스트림 연산 수행
	.map(...)
```

### takeWhile
takeWhile은 해당 조건문을 충족시킬 때 까지의 데이터를 이후 스트림에 넘긴다.
```java
IntStream.range(0,10)
	.takeWhile(i -> i < 4) // 0, 1, 2, 3
```

### dropWhilte
dropWhile은 해당 조건문을 만족하는 동안만 요소를 무시한 뒤, 나머지 요소들을 다음 스트림에 넘긴다.
```java
IntStream.range(0,10)
	.takeWhile(i -> i < 4) // 4, 5, ... 9
```


## 최종 연산
###  forEach
각 요소들에 주어진 Consumer를 실행
```java
IntStream
	.range(1, 100)
	.forEach(System.out::println);
```

### max
가장 큰 요소를 Optional 타입으로 가져온다.
```java
int res = IntStream.range(1, 100)
	.max().getAsInt(); //100
```

### sum
스트림 요소의 합을 반환한다.
```java
IntStream.rangeClosed(0, 100).sum(); // 5050
```

### collect

### allMatch
스트림의 모든 요소가 Predicate를 만족하는지에 대한 결과를 boolean으로 반환한다.
```java
IntStream.range(1, 10)
	.allMatch(i -> i > 0) // true
```

### anyMatch
```java
IntStream.range(1, 10)
	.anyMatch(i -> i % 2 == 0) // truie
```

### reduce
주어진 BiFunction으로 값을 접어나감
```java
IntStream.range(1, 10)
	.reduce((prev, cur) -> {
		System.out.printf("%d %d\n", prev, cur); 
		return prev * cur;
	})
	.getAsInt(); // 362880
// 1 2
// 2 3
// 6 4
// 24 5
//... 
```

seed값을 지정해 줄 수 있다. seed를 지정해 준 경우 OptionalInt가 반환되지 않으므로 바로 값을 받을 수 있다.
```java
IntStream.range(1, 10) 
	.reduce(2, (prev, cur) -> prev * curr); // 초기값을 2로 지정
```