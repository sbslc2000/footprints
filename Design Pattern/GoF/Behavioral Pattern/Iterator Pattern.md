---
상위 링크: "[[Behavioral Pattern]]"
---
# Iterator Pattern

## Purpose
Iterator Pattern의 목적은 컬렉션(집합체) *Aggregate Object*의 내부 구조를 노출하지 않고도, 그 안에 들어있는 요소들에 순차적으로 접근할 수 있는 방법을 제공하는 것이다. 이를 통해 다양한 구조의 컬렉션(배열, 연결리스트, Map,...)의 요소들을 일관된 방식으로 순회할 수 있다.

## Use When
* 하나의 컬렉션에 대해 여러개의 커서가 필요할 때
* 서로 다른 컬렉션에 대해 일관된 방식으로 접근하고 싶을 때

## Class Diagram
![](https://i.imgur.com/XWXSozr.png)
