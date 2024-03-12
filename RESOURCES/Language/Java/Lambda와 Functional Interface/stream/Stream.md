---
상위 링크: "[[Java]]"
---
# Stream

## 장점

* 일련의 데이터를 연속적으로 가공하는데에 유용하다.
* 중간과정이 밖으로 드러나지 않으므로, 변수등이 만들어지지 않는다. -> 이를 통해 오류나 예외 사항으로부터 안전할 수 있음
* 배열, 컬렉션, I/O 등에서 동일한 프로세스로 가공할 수 있다.
* 함수형 프로그래밍을 위한 다양한 기능을 제공 - 원본을 수정하지 않는다.
* 가독성 향상
* 병렬처리 가능


## 주의할 점

Stream의 최종 메서드는 여러번 호출할 수 없다. 한번 꺼내면 끝이다.
```java
Stream<Hello> stream = Arrays.stream(hello);

Hello[] arr = stream.toArray(); // 이건 가능
Hello[] arr = stream.toArray(); // 이건 불가능 (stream이 이미 처리되었음.)
```
## 예시

리스트의 내용을 홀수만 골라내고, 정렬하여, 하나의 문자열로 합치는 절차적인 예시이다.
```java
List<Integer> int0To9 = new ArrayList<>(
	Arrays.asList(5, 2, 0, 8, 4, 1, 7, 9, 3, 6);
);
//홀수만 골라낸 다음 정렬하여 '1,3,5,7,9'의 문자열로 만들어보자

List<Integer> odds = new ArrayList<>();
for (Integer i : int0To9) {
	if (i % 2 == 1) odds.add(i);
}
odds.sort(Integer::compare);

List<String> oddsStrs = new ArrayList<>();
for(Integer i : odds) {
	oddsStrs.add(String.valueOf(i));
}

String oddsStr = String.join(", ", oddsStrs);
```

이를 stream을 통해 정리하면 다음과 같다.
```java
String result = int0To9
	.stream()
	.filter(i -> i % 2 == 0)
	.sorted(Integer::compare)
	.map(String::valueOf)
	.collect(Collectors.joining(", "));
```