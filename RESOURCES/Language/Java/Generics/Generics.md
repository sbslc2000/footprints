# Generic Programming 
> Generic programming centers around the idea of **abstracting from concrete, efficient algorithms** to obtain **generic algorithms** that can be combined with **different data representations** to produce a wide variety of useful software.

Generic이란 구체적인 타입에 대한 정보를 타입 정의 시점이 아닌 타입의 인스턴스화 시점에 전달함으로써 하나의 타입으로 여러 가지 타입을 표현하는 프로그래밍 기법이다. 로직을 추상화하여 여러 타입에 적용시킬 수 있게 한다.

Java의 Generic을 사용함을 통해 다양한 타입의 객체를 다루는 메서드나 클래스에 대해 **컴파일 타임 체크를 가능하게 하여 타입 안정성을 높이고 형 변환의 번거로움을 줄여준다.**

## 타입 안정성? 형 변환의 번거로움?
타입 안정성과 형 변환의 번거로움을 코드레벨로 설명하면 다음과 같다.

### Generic을 사용하지 않는다면

NoGenericList는 데이터를 저장할 수 있는 아주 간단한 리스트이다. 파라미터로 주어진 정보를 저장하고, 이를 반환해주는 로직을 갖고있다. 데이터는 Object 형태로 받고 있어 객체 타입에 상관없이 저장할 수 있다.
```java
class NoGenericList {  
    Object[] elements;  
  
    public List(Object ... args) {  
        elements = new Object[args.length];  
        for (int i = 0; i < args.length; i++)  
            elements[i] = args[i];  
    }  
  
    public Object get(int i) {  
        return elements[i];  
    }  
}
```

이 리스트를 사용하여 Dog과 Cat을 저장할 수 있다. 이후 값을 가져올 땐 Object를 반환하므로, 이 객체로부터 이름을 가져오고자 할 때에는 각각의 클래스로 명시적 형변환을 해주어야 한다.
```java
void test() {  
    List dogList = new NoGenericList(  
            new Dog("바둑이"),  
            new Dog("뽀삐"),  
            new Dog("멍멍이"));  
      
    List catList = new NoGenericList(  
            new Cat("나비"),  
            new Cat("야옹이"),  
            new Cat("냥냥이"));  
      
    Dog dog = (Dog) dogList.get(0);  
    System.out.println("dog.getName() = " + dog.getName());  //바둑이
    Cat cat = (Cat) catList.get(0);  
    System.out.println("cat.getName() = " + cat.getName());  //나비
}
```

만약 이 과정에서 개발자가 실수한다면 어떤 일이 발생할까? 아래는 리스트를 혼동하여 형변환이 잘못 이루어진 경우이다.
```java
Dog dog1 = (Dog) dogList.get(0);  
System.out.println("dog2.getName() = " + dog1.getName());  
Dog dog2 = (Dog) catList.get(1);  
System.out.println("dog2.getName() = " + dog2.getName());
```
이 경우 자바 컴파일러는 어떠한 오류도 뱉어내지 않으며, 런타임에서야 ClassCastException이 발생한다.

결국 Generic을 사용하지 않는다면 하나의 로직을 추상화한 클래스를 사용하기 위해 매번 명시적 형변환을 해줘야 한다는 번거로움이 있으며, 이 과정에서 실수가 발생했을 때 컴파일러가 에러를 잡아주지 못하기 때문에 타입 안정성이 떨어지게 된다.

### Generic을 사용한다면

Generic을 사용하여 변경한 코드는 다음과 같다.
```java
class GenericList<T> {  
    T[] elements;  
  
    public GenericList(T ... args) {  
        elements = (T[]) new Object[args.length];  
        for (int i = 0; i < args.length; i++)  
            elements[i] = args[i];  
    }  
  
    public T get(int i) {  
        return elements[i];  
    }  
}
```

각각의 list에게는 타입 정보가 있어서 컴파일러가 직접 캐스팅을 수행해주며, 이 과정에서 타입 체크를 수행하여 타입이 맞지 않는 경우 컴파일 레벨에서 에러를 발생시킨다. 
![](https://i.imgur.com/MQ2b2xm.png)

## Type Erasure
[[Type Erasure]]