# 개요

java.lang.Object는 모든 클래스의 조상이며, 어떠한 필드도 없이 메서드들만 갖고 있다. 

```java
@IntrinsicCandidate
public final native Class<?> getClass();
```
* @IntrinsicCandidate : HotSpot VM 에 의한 최적화
* native : C, C++ 등 다른 언어로 작성된 코드를 호출

## toString()
기본적으로는 클래스명과 해시값을 반환하게 되어있으며, println() 메서드로 객체를 출력할 때 이 메서드의 결과값을 출력한다.
toString() 오버라이딩을 통해 println() 메서드로 출력될 때의 형태를 변경할 수 있다.

## equals()
기본적으로는 == 연산과 동일하게 작동한다. (주소값 비교)
인스턴스의 내용을 비교해주려면 해당 클래스에 equals()를 오버라이딩하여 내용 비교 로직을 작성해줄 수 있다.

## hashCode()
기본적으로는 객체의 고유한 주소값을 반환한다.

String 메서드는 hashCode()를 오버라이딩하여 문자열만 같다면 주소가 달라도 true를 반환하게 되어있다.
