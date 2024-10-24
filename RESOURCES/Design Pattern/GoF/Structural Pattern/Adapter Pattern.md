---
상위 링크: "[[Structural Pattern]]"
---
# Adapter Pattern

## Purpose

> Permits classes with different interfaces to work together by creating a common object by which they may communicate and interact

Adapter Pattern의 목적은 호환되지 않는 인터페이스를 가진 클래스들이 함께 작동할 수 있도록, 중간에 어댑터를 두어 인터페이스를 변환해주는 것이다. 이 패턴은 기존의 클래스를 변경하지 않고, 서로 다른 인터페이스를 가진 클래스들이 협력할 수 있도록 돕는다.

## Use When
* 새로운 시스템이 레거시 시스템과 호환성을 유지해야할 때
* 서로 다른 라이브러리를 통합하고자 할 때
* 클래스의 인터페이스가 요구사항과 맞지 않을 때


## Participant
* Target Interface : 클라이언트가 사용할 인터페이스
* Adaptee 클래스 : 기존 인터페이스를 제공하는 클래스
* Adapter 클래스 : Target Interface를 구현하며, Adaptee 객체를 사용하여 요청을 변환


## Class Diagram

### Object Adapter 
![](https://i.imgur.com/JQVHEj7.png)

### Class Adapter
![](https://i.imgur.com/w9uUUEc.png)
