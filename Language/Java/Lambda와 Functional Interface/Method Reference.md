---
상위 링크: "[[Java Lambda]]"
---
# 메소드 참조Method Reference
람다식이 어떤 메소드 하나만 호출할 때 코드를 간단하게 만든다  -> 람다식과 메서드의 의미가 사실상 같을 때
사용하고자 하는 메서드가 함수형 인터페이스와 인자, 리턴값 구성이 동일할 때 사용한다.

```java
Function<Integer,String> intToStrLD = (i) = String.valueOf(i);
Function<Integer,String> intToStrMR = String::valueOf;
```