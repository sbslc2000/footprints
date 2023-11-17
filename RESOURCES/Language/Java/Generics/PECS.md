# PECS

PECS는 Producer Extends Consumer Super의 약자로, Java Generic의 중요한 개념 중 하나이다.
PECS는 생산(조회)하는 측에서는 Extends를, 소비(저장,수정 등의 기능)하는 측에서는 super를 사용한다는 의미이며, 이것을 이해하기 위해서는 와일드카드에 대한 개념을 알아야 한다.

> [!info] 
> 직관적으로 느끼기에는 데이터를 저장 및 수정하는 기능을 수행하는 측을 Producer로, 데이터를 조회하는 기능을 수행하는 측을 Consumer로 보는 것이 알맞아 보인다.
## Invariance
불공변성*Invariance*은 서로 다른 제너릭 타입간 계층 관계가 존재하지 않는다는 Generics의 특성이다.
```java
class Dog extends Animal {
	...
}

class Cat extends Animal {
	...
}

...
	GenericList<Animal> dogList = new GenericList<Dog>(); //compile error
	GenericList<Animal> catList = new GenericList<Cat>(); //compile error
```

같은 이유로 파라미터 타입도 상속관계를 처리하지 못한다.
```java
void print(GenericList<Animal> animalList) {
	//대충 animal을 출력하는 코드
}

...
	GenericList<Dog> dogList = new GenericList<>();
	print(dogList); //compile error
```

## 와일드카드 \<?>
불공변성에 의한 문제를 해결하기 위해서 Generics에서는 와일드카드를 지원한다.
```java
GenericList<?> list = new GenericList<Dog>(); //?은 모든 타입을 지원
GenericList<? extends Animal> catList = new GenericList<Cat>(); // Animal과 하위의 타입을 지원
GenericList<? super Dog> dogList = new GenericList<Dog>(): // Dog과 상위 타입을 지원
```

와일드카드와 함께 extends, super 키워드를 사용하면 받을 수 있는 타입 인자에 제약을 줄 수 있다.
제너릭스에 와일드카드와 함께 extends 키워드를 사용하는 것을 **Upper Bounded Wildcard**, super 키워드를 사용하는 것을 **Lower Bounded Wildcard**라고 부른다.
