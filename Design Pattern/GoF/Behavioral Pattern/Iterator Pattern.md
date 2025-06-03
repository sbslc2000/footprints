---
상위 링크: "[[Behavioral Pattern]]"
---
# Iterator Pattern

## Purpose
Iterator Pattern의 목적은 컬렉션(집합체)(Aggregate Object)의 내부 구조를 노출하지 않고도, 그 안에 들어있는 요소들에 순차적으로 접근할 수 있는 방법을 제공하는 것이다. 이를 통해 다양한 구조의 컬렉션(배열, 연결리스트, Map,...)의 요소들을 일관된 방식으로 순회할 수 있다.

## Use When
* 하나의 컬렉션에 대해 여러개의 커서가 필요할 때
* 서로 다른 컬렉션에 대해 일관된 방식으로 접근하고 싶을 때

## Class Diagram
![](https://i.imgur.com/XWXSozr.png)

## Built-in
여러 프로그래밍 언어는 Iterator Pattern을 언어 차원에서 지원하거나, 기본 라이브러리에서 이터레이터 기능을 내장하여 제공한다. 이로 인해 개발자는 직접 반복자를 구현하지 않고도 일관된 방식으로 다양한 컬렉션을 순회할 수 있다.

### **JavaScript의 Built-in Iterator**
JavaScript에서는 ES6부터 Symbol.iterator를 통해 이터러블 프로토콜을 정의할 수 있게 되었다. Array, Set, Map, String 등 대부분의 기본 자료형은 이터러블(Iterable)이며, 내부적으로 iterator를 내장하고 있다.

```javascript
const arr = [10, 20, 30];
const iterator = arr[Symbol.iterator]();

console.log(iterator.next()); // { value: 10, done: false }
console.log(iterator.next()); // { value: 20, done: false }
console.log(iterator.next()); // { value: 30, done: false }
console.log(iterator.next()); // { value: undefined, done: true }

```
위 코드처럼 Symbol.iterator를 통해 명시적으로 이터레이터를 꺼내어 순회할 수도 있고, `for...of` 구문을 통해 추상화된 방식으로 순회할 수도 있다.
```javascript
for (const value of arr) {
  console.log(value); // 10, 20, 30
}
```

### **Java의 Built-in Iterator**
Java에서는 java.util.Iterator 인터페이스를 통해 이터레이터 패턴을 지원한다. List, Set, Queue 등 대부분의 컬렉션은 이 인터페이스를 기반으로 이터레이터를 제공하며, hasNext()와 next() 메서드를 통해 요소에 순차적으로 접근할 수 있다.
```java
List<String> list = Arrays.asList("a", "b", "c");
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String item = iterator.next();
    System.out.println(item);
}
```