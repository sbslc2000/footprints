---
상위 링크: "[[Design Principle]]"
---
# Law of Demeter

Law of Demeter는 소프트웨어 설계에서 객체간의 의존성을 줄이고, 객체 내부 구조에 대한 결합도를 낮추기 위한 원칙이다. 이 원칙은 "Talk only to your immediate friends"라는 개념을 따르며, 객체가 다른 객체와 상호작용할 때 **직접적인 관계가 있는 객체**와만 통신해야 한다고 규정한다.

여기서 직접적인 관계가 있는 객체는 다음과 같다.
1. 자기 자신
2. 메서드의 매개변수로 전달된 객체
3. 자시 자신의 멤버변수로 가지고 있는 객체
4. 자기 자신의 메서드에서 생성한 객체
5. 전역변수가 아닌, 클래스의 멤버 객체

이를 잘 따르면, 한 줄의 코드에서는 객체를 `.`으로 2번 이상 연결하여 다른 객체에 접근하는 일이 줄어든다.

Law of Demeter를 따르지 않는 코드는 다음과 같다.
```java
someMethod(User customer) {
	int price = customer.getOrder().getProduct().getPrice();
	...
}
```
위와 같은 코드는 메서드에서 다룰 수 있는 customer를 시작으로 그 내부 객체들에 접근하는데, 위 규칙에 따르면 User의 getOrder() 까지는 메서드의 매개변수로 전달된 객체이므로 허용되지만 getProduct()는 Order의 메서드이므로 허용되지 않는다.

```java
... int price = customer.getOrderPrice();
```
위와 같이 작성해놓는다면, 이제 해당 메서드는 파라미터로 들어온 객체만을 알고 그 내부 구현은 모르므로 다른 클래스에 대한 의존성은 줄어들게 된다.