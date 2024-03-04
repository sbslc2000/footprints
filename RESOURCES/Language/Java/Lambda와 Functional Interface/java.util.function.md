---
상위 링크: "[[Java Lambda]]"
---
# java.util.function 패키지
람다식을 사용하기 위해서는 각 상황에 맞는 여러 함수형 인터페이스를 어딘가에 선언해야한다. java.util.Function은 자주 사용하는 인터페이스를 만들어 적재적소에 사용할 수 있게 만들었다.

| Functional Interface | Method | params | return |
| ---- | ---- | ---- | ---- |
| Runnable (java.lang) | run |  |  |
| Supplier\<T> | get |  | T |
| Consumer\<T> | accept | T |  |
| BiConsumer<T, U> | accept | T, U |  |
| Function<T, R> | apply | T,  | R |
| BiFunction<T, U, R> | apply | T, U | R |
| Predicate\<T> | test | T | boolean |
| BiPredicate<T, U> | test | T, U | boolean |
| UnaryOperator\<T> | apply | T,  | T |
| BinaryOperator\<T> | apply | T, T | T |

