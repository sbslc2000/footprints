# Value Object

> **“값 객체는 식별자 없이, 그 값 자체로 의미를 가지는 불변의 객체다.”**
> – 에릭 에반스, 『Domain-Driven Design』

값 객체, 혹은 값 타입(Value Object, Value Type)이란 고유 식별자가 없고, 그 값 자체가 객체의 본질을 결정하는 도메인 객체이다.
```java
public class Address {
    private final String city;
    private final String street;
    private final String zipCode;

    public Address(String city, String street, String zipCode) {
        this.city = city;
        this.street = street;
        this.zipCode = zipCode;
    }

    // equals와 hashCode는 모든 필드 기반으로
}
```

## Immutable
```java
public class Money {
	private int value;
	public Money add(Money money) {
		return new Money(this.value + money.value);
	}
}
```
값 객체의 데이터를 변경할 때는 기존 데이터를 변경하기보다는 변경한 데이터를 갖는 새로운 값 객체를 생성하는 방식을 선호한다. 이러한 타입을 불변 타입(Immutable Type)이라고 부르며, 부작용이 발생하지 않으므로 안전한 코드를 작성하는데에 도움을 준다.
