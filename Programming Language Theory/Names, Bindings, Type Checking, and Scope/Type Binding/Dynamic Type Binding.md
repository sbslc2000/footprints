---
상위 개념: "[[Type Binding]]"
---
# Dynamic Type Binding
정적 타입 바인딩에서 변수의 타입은 선언문이나 변수명의 컨벤션, 혹은 추론에 의해 컴파일 타임에 결정되지만, 동적 타입 바인딩에서는 변수에 어떤 값이 할당되는 순간 바인딩이 발생한다.

```python
count =  1
count = 3.0
count = [1,2,3]
```
예를 들어 위 파이썬 예제에서 count라는 변수는 각각의 값이 할당되는 순간 그 값에 대한 타입을 부여받게 된다. 해당 변수의 값이 저장되는 메모리 영역 역시 값이 할당되는 시점에 바인딩되는데, 이는 실행 이전에는 해당 변수에 필요한 공간을 알 수 없기 때문이다. 

동적 타입 바인딩은 코드를 유연하게 작성하게 도와주지만, 타입이 런타임에 결정되기 때문에 컴파일 타임에 에러를 미리 감지할 수 없다. 또한 런타임에 변수에 대한 연산이 가능한지 매번 타입 체킹을 해주어야 해서 추가적인 비용이 발생한다.

일반적으로 동적 타입 바인딩은 인터프리터 언어에서 사용하는데, 이는 몇가지 이유가 존재한다.

1. 컴파일러는 실행 전 바이너리 명령어를 만들어야 하는데, 타입이 동적으로 정해진다면 컴파일 타임에 명령어를 만들어 낼 수 없다.
2. 동적 타입 바인딩은 그 자체로 비용이 큰데(타입 체킹, 동적 메모리 할당), 인터프리터 언어는 이미 인터프리팅에 더 큰 시간을 쓰므로 비용이 자연스레 묻힌다.

