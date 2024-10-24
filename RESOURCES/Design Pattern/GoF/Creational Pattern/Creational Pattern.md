---
상위 링크: "[[Design Pattern]]"
---
# Creational Pattern
생성 패턴은 클라이언트 코드에서 new operator를 명시적으로 사용하지 않고 객체를 생성하도록 도와주는 패턴의 모음이다.

New operator 뒤에는 항상 concrete한 클래스 이름이 오기 때문에, 클라이언트 코드에서 new를 명시적으로 사용하는 것은 프로그램을 변경에 어렵게 만든다. Creational pattern은 new operator를 클라이언트 코드에서 숨기고 객체를 생성해서 전달해주는 역할을 한다.

Factory Method, Abstract Factory, Singleton, Builder, Prototype 패턴은 대표적인 Creational Pattern이다.