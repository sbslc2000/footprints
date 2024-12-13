---
상위 링크: "[[Generics]]"
---
# Generic Method
메서드의 선언부에 제너릭 타입이 선언된 메서드를 제너릭 메서드라고 한다. 
```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```

이때 제너릭 클래스에 정의된 타입 매개변수와 제너릭 메서드에 정의된 타입 메개변수는 별개의 것으로, 같은 문자를 사용하더라도 다른 타입으로 취급된다. 또한 제너릭 클래스가 아닌 곳에서도 정의될 수 있다.


제너릭 메서드를 사용하여 다음 메서드를 아래와 같이 바꿀 수 있다.
```java
static Juice makeJuice(FruitBox<? extends Fruit> box) {
	...
}

static <? extends Fruit> Juice makeJuice(FruitBox<T> box) {

}
```

이때 메서드를 호출할 때는 타입 변수에 타입을 대입해야하지만, 일반적으로 컴파일러가 타입을 추정할 수 있기 때문에 생략해도 된다.
```java
FruitBox<Apple> appleBox = new FruitBox<Apple>();

Juicer.<Apple>makeJuice(appleBox); //타입 명시
Juicer.makeJuice(appleBox); // 타입 추론
```