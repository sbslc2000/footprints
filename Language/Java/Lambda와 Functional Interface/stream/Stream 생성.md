---
상위 링크: "[[Stream]]"
---

## 스트림 생성

1. Wrapper Array로부터 스트림 생성하기
```java
Integer[] integerArray = {1, 2, 3, 4, 5};
Stream<Integer> fromArray = Arrays.stream(integerArray);
```

2. primitive array로부터 스트림 생성하기
```java
int[] intArray = {1, 2, 3, 4, 5};
IntStream fromIntArray = Arrays.stream(intAry);
```
이 경우 Stream<>이 아닌 원시값 전용 스트림이 만들어진다. 하지만 제공하는 기능은 동일하다.

3. 값들로부터 직접 생성하는 방법
```java
IntStream withInts = IntStream.of(1, 2, 3, 4, 5);

Stream<Integer> withIntegers = Stream.of(1, 2, 3, 4, 5);
```

4. Collection으로부터 생성하는 방법
```java
List<Integer> intAryList = new ArrayList<>(Arrays.asList(0,1,2,3));

Stream<Integer> fromCollection = intAryList.stream();
```

Map의 경우 엔트리의 스트림을 사용해야한다.
```java
Map<String, Hello> map = new HashMap<>();
map.entrySet().stream().toArray();
```

5. Builder 사용
```java
Stream.Builder<Character> builder = Stream.builder();
builder.accept('스');

Stream<Character> withBuilder = builder.build();
```

6. concat
```java
Stream<Integer> stream1 = Stream.of(11,22,33);
Stream<Integer> stream2 = Stream.of(44,55,66);

Stream<Integer> concatenated = Stream.concat(stream1, stream2);
```

7. iterator로 생성
```java
Stream<Integer> withIter1 = Stream
	.iterate(0, i -> i + 2)
	.limit(10); // 0 2 4 6 8 10 12 14 16 18
```

8. range 사용
```java
IntStream fromRange1 = IntStream.range(10, 20); // 20 미포함
IntStream fromRange2 = IntStream.rangeClosed(10, 20); // 20 포함

Stream<Integer> integer = fromRange2.boxed(); // boxing
```

9. Random 사용
```java
IntStream randomInts = new Random().ints(5, 0, 100); //0 부터 100까지의 수 중 5개를 가져오기
```

10. File로부터 생성
```java
Stream<String> fromFile;
Path path = Paths.get("/src...")

fromFile = Files.lines(path);
```
