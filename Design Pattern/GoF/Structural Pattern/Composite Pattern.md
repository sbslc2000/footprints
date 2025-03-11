---
상위 링크: "[[Structural Pattern]]"
---
# Composite Pattern

## Purpose
> *Facilitates the creation of object hierarchies where each object can be treated independently or as a set of nested objects through the same interface*

Composite Pattern의 목적은 객체를 트리 구조로 구성하여, 개별 객체와 복합 객체를 **동일한 방식으로 처리**할 수 있게 함으로써 코드의 일관성과 유연성을 높이는 것이다.

## When to Use
* 객체의 계층적 표현이 필요할 때
* 객체들이나 객체의 조합을 일관되게 다루고 싶을 때

## Usage

### xUnit
xUnit 라이브러리들은 TestSuite과 TestCase를 같은 인터페이스로 다루어, 테스트의 묶음이든 개별 테스트든 동일한 인터페이스로 테스트를 수행시킨다.

```java
public interface Test {  
    void run(TestResult result);  
}

public class TestCase implements Test {  
    protected final String name;  
  
    /**  
     * 테스트 대상 메서드 이름을 받는다.  
     *     * @param name  
     */  
    public TestCase(String name) {  
        this.name = name;  
    }  
  
    public void run(TestResult result) {  
        result.testStarted();  
  
        //테스트 메서드 호출 전에 setUp이 호출되어야 한다.  
        setUp();  
  
        try {  
            Method method = getClass().getMethod(name);  
            method.invoke(this);  
        } catch (NoSuchMethodException | IllegalAccessException | InvocationTargetException e) {  
            result.testFailed();  
        }  
  
        tearDown();  
    }  
  
    public void tearDown() {  
  
    }  
  
    public void setUp() {  
    }  
}

public class TestSuite implements Test {  
  
    List<Test> tests = new ArrayList<>();  
  
    public TestSuite(Class<? extends Test> testClass) {  
        Arrays.stream(testClass.getDeclaredMethods())  
                .filter(m -> m.isAnnotationPresent(src.xunit.annotation.Test.class))  
                .forEach(m -> {  
                            try {  
                                add(testClass.getConstructor(String.class).newInstance(m.getName()));  
                            } catch (Exception e) {  
                                throw new RuntimeException();  
                            }  
                        }  
                );  
    }  
  
    public TestSuite() {  
    }  
  
    public void add(Test test) {  
        tests.add(test);  
    }  
  
    public void run(TestResult result) {  
        tests.forEach(t -> t.run(result));  
    }  
}
```

## Class Diagram
![](https://i.imgur.com/KmuPQTw.png)
위 방법은 단말노드와 중간노드를 구별하지 않는 방법이다. 
이 방법은 중간노드와 단말노드 각각에서만 수행되어야 하는 메서드를 구분하지 않으므로 안정성이 떨어질 수 있다.
![](https://i.imgur.com/aRdYCKB.png)
위 방법은 단말노드와 중간노드의 공통 메서드만 Component에 취급하는 방식이다. 안정성은 높지만, 모든 객체를 일관되게 다루지 못하고 downcast를 수행해야 할 수도 있다.

## Example Source Code
[design-pattern-example/src/main/java/io/github/sbslc2000/composite at main · sbslc2000/design-pattern-example · GitHub](https://github.com/sbslc2000/design-pattern-example/tree/main/src/main/java/io/github/sbslc2000/composite)