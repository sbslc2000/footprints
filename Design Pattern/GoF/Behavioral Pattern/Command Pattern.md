---
상위 링크: "[[Behavioral Pattern]]"
---
# Command Pattern

## Purpose

> Command pattern encapsulates a request as an object. This allows the request to be handled in traditionally object based relationships such as queuing and callback.

Command Pattern의 목적은 요청이나 작업을 객체로 캡슐화하여, 요청을 파라미터화하고 저장하거나 로깅하며, 요청의 실행을 큐에 넣거나 실행 취소(undo) 기능을 제공할 수 있도록 하는 것이다. 이 패턴은 명령을 객체로 추상화함으로써 객체의 장점을 누릴 수 있게 해준다.

## Use When
* 요청이 생성되고, 보관되고, 실행되는 것의 시점을 다르게 하고 싶을 때
* 요청에 대한 기록을 남기고 싶을 때
* 요청과, 그것을 실제로 수행하는 객체를 분리하고 싶을 때

## Participant
* Command Interface : 요청에 대한 추상화
* ConcreteCommand: 실제로 요청을 실행하는 명령 객체
* Receiver : 실제로 명령을 수행하는 객체. 
* Invoker: Command 객체를 호출하여 명령을 실행

## Class Diagram

![](https://i.imgur.com/dcTjZjN.png)
