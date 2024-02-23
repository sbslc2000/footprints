---
상위 개념: "[[Generics]]"
---
# Type Erasure
Generic을 사용하기 위해 부가적으로 들어간 소스코드들은 바이트코드 레벨에서 모두 제거된다.

### 런타임에 제너릭 타입의 정보는 존재하지 않는다. 
제너릭을 사용한 코드를 디컴파일 해보면, 마치 제너릭을 사용하지 않고 리스트를 사용했을 때와 비슷한 형태로 코드가 구성되어있음을 확인할 수 있다.  
```java
void genericTest() {  
    GenericList dogList = new GenericList(new Dog[]{new Dog("바둑이"), new Dog("뽀삐"), new Dog("멍멍이")});  
    GenericList catList = new GenericList(new Cat[]{new Cat("나비"), new Cat("야옹이"), new Cat("냥냥이")});  
    Dog dog1 = (Dog)dogList.get(0);  
    System.out.println("dog1.getName() = " + dog1.getName());  
    Cat cat1 = (Cat)catList.get(1);  
    System.out.println("cat1.getName() = " + cat1.getName());  
}
```

또한 .getClass()를 통해 클래스 정보를 출력해보면 catList와 dogList 간 차이가 없는 것을 확인할 수 있다.
```java
@Test  
void genericPrintClass() {  
    GenericList<Dog> dogList =  new GenericList<>(  
            new Dog("바둑이"),  
            new Dog("뽀삐"),  
            new Dog("멍멍이"));  
  
    GenericList<Cat> catList = new GenericList<>(  
            new Cat("나비"),  
            new Cat("야옹이"),  
            new Cat("냥냥이"));  
  
    System.out.println(dogList.getClass());  
    System.out.println(catList.getClass());  
    System.out.println(dogList.getClass() == catList.getClass());  
}
```

```
#출력결과
class com.example.demo.parameterizedType.GenericTest$GenericList
class com.example.demo.parameterizedType.GenericTest$GenericList
true
```
이렇게 제너릭과 관련된 소스코드 상의 정보가 컴파일러로부터 제거되는 것을 Type Erasure이라고 하며, 이러한 것이 수행되는 이유는 자바 기존 코드(제너릭이 없던 시절)와의 호환성 때문이다.

결국 제너릭은 런타임 실행 코드에는 특별한 문법적인 요소가 추가되는 것은 아니지만, 컴파일 타임에 개발자에게 편의 기능을 제공하는 방식으로 동작하며 이러한 특징 때문에 syntactic sugar로 분류되곤 한다.

### 하지만 일부 경우 제너릭 타입 정보가 런타임에 유지되기도 한다.
1. 제너릭 클래스를 상속받아서 클래스를 만드는 경우
2. 제너릭 클래스를 포함하여 클래스를 만드는 경우
```java
//제너릭 상속
public DogInheritedRepository extends List<Dog> {
	...
}

//제너릭 포함
public DogCompositedRepository {
	private List<Dog> list;
}
```

위 경우들은 리플렉션 기능을 통해 제너릭 타입의 정보를 가져올 수 있습니다.
```java
DogInheritedRepository dogRepository1 = new DogInheritedRepository(  
        new Dog("바둑이"),  
        new Dog("뽀삐"),  
        new Dog("멍멍이"));  
  
DogCompositedRepository dogRepository2 = new DogCompositedRepository(  
        new Dog("바둑이"),  
        new Dog("뽀삐"),  
        new Dog("멍멍이"));  
  
ParameterizedType type1 = (ParameterizedType) dogRepository1.getClass().getGenericSuperclass();  
System.out.println("type1.getActualTypeArguments() = " + type1.getActualTypeArguments()[0]);  
ParameterizedType type2 = (ParameterizedType) dogRepository2.getClass().getDeclaredFields()[0].getGenericType();  
System.out.println("type2.getActualTypeArguments() = " + type2.getActualTypeArguments()[0]);
```

```
#결과
type1.getActualTypeArguments() = class com.example.demo.generic.GenericTest$Dog
type2.getActualTypeArguments() = class com.example.demo.generic.GenericTest$Dog
```
