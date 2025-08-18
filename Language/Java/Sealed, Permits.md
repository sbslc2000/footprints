---
상위 개념: "[[Java]]"
---
# Sealed, Permits
Java의 sealed class와 permits 문법은 Java 17버전부터 정식적으로 도입된 기능으로, 클래스 상속 계층을 제한하는 용도로 사용된다. 이를 통해 클래스의 확장 가능성을 명확히 제어할 수 있고, 유지보수성 및 보안성을 높이는데에 도움이 된다.

## 사용법
sealed 키워드를 붙이면 해당 클래스를 상속할 수 있는 클래스의 목록을 제한할 수 있다. 즉, 특정 클래스들만이 이 클래스를 상속하도록 허용한다.
```java
public abstract sealed class Shape 
	permits Circle, Rectangle, Square {
	public abstract double area();	
}
```
permits 키워드 뒤에는 상속이 허용되는 서브클래스 목록을 명시한다. 이 목록에 없는 클래스는 Shape를 상속할 수 없으며, 서브클래스는 반드시 final, sealed, non-sealed 중 하나로 선언해야한다.

## 서브클래스 선언 방식
상속이 허용된 클래스들은 반드시 다음 중 하나로 선언되어야 한다.

1. final
더 이상의 상속을 허용하지 않음
```java
public final class Circle extends Shape {
    @Override
    public double area() { return Math.PI * 1 * 1; }
}
```

2. sealed
다시 다른 하위 클래스들에게 상속을 제한 
```java
public sealed class Rectangle extends Shape
    permits FilledRectangle, TransparentRectangle {
    @Override
    public double area() { return 10 * 20; }
}
```

3. non-sealed
더 이상 상속을 제한하지 않고, 자유롭게 상속 가능
```java
public non-sealed class Square extends Shape {
    @Override
    public double area() { return 5 * 5; }
}
```