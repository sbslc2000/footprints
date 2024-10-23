---
상위 링크: "[[Creational Pattern]]"
---

# Singleton Pattern

## Purpose
> Ensures that only one instance of a class is allowed within a system

Singleton Pattern은 시스템 내에 특정 클래스의 인스턴스가 1개만 유지되도록 보장하기 위해 사용한다.

기본적으로는 시스템 내에 인스턴스가 오직 한 개만 필요하거나, 한 개로 충분할 때 사용한다. 싱글톤 패턴의 매커니즘을 약간만 조정하면 인스턴스의 개수를 상수개로 제한하는 패턴으로 사용할 수도 있다.

## 기본 구조
```java
public class Singleton {
	private static Singleton uniqueInstance;

	private Singleton() {}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}

		return uniqueInstance;
	}

}
```

1. private 생성자를 통해 외부에서 임의로 인스턴스를 생성하는 동작을 막아야 한다.
2. uniqueInstance 변수, getInstance() 메서드
	1. 인스턴스를 하나만 만들어야 하기 때문에 클래스레벨에서 유지할 수 있는 공간 사용
	2. 이미 만들어져있다면 그 객체를 반환, 없다면 객체를 생성해서 반환
	3. Static이기에 인스턴스가 없더라도 호출 가능

## Class Diagram

![](https://i.imgur.com/uqjWNQA.png)

# Thread-safe Singleton Pattern
멀티 쓰레드 환경에서는 instance가 2번 이상 생성될 가능성이 열려있다. 상황에 따라서는 하나의 쓰레드에서 반환한 인스턴스와 다른 쓰레드에서 반환한 인스턴스가 다를 수도 있다.

다음 해결책들은 Java 언어에 국한된 해결법으로, 다른 언어에서는 다른 해결법을 사용해야할 수 있다.

## Option 1 : synchronized keyword
```java
public class Singleton {
	private static Singleton uniqueInstance;

	private Singleton() {}

	public static synchronized Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}

		return uniqueInstance;
	}

}
```
synchronized 키워드를 사용하여 getInstance 메서드 내부에 동시에 접근할 수 있는 쓰레드 개수를 1개로 제한하는 방법이 있다.

이 방법은 싱글톤을 온전히 보장하지만, lock의 범위가 커 성능적인 단점이 있다. 모든 쓰레드들은 getInstance를 호출할 때마다 경쟁 조건에 걸린다. 물론 경쟁조건이 별로 없을만한 상황이라면 좋은 방법일 수 있다.

## Option 2 : Global Variable
```
public class Singleton {
	private static Singleton uniqueInstance = new Singleton();

	private Singleton() {}

	public static synchronized Singleton getInstance() {
		return uniqueInstance;
	}

}
```

이 방법은 Option 1과 다르게 쓰레드들이 아무런 경쟁 없이 인스턴스를 획득할 수 있으며, 유일성도 보장된다. 하지만 로드 시 무조건 초기화가 되므로 인스턴스를 사용하지 않는 환경에서도 로딩되므로 불필요한 비용을 증가시킬 수도 있다.

따라서 이 방법은 해당 객체가 거의 무조건 사용된다는 가정하에는 좋은 방법일 수 있다. 별다른 런타임 오버헤드도 없으며 경쟁조건도 발생하지 않기 때문이다.

## Option 3: Double-checked Locking

Option1은 Lock의 범위가 너무 넓다는 부분이 문제였다. Lock의 범위를 축소시키면 어떨까?

생성만 1번 수행되게 만들어야 하니 new Singleton()에 대해서만 경쟁을 시키도록 만들어 볼 수 있다.

```java
public class Singleton {
	private static Singleton uniqueInstance;

	private Singleton() {}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized(Singleton.class) {
				uniqueInstance = new Singleton();
			}
		}
		return uniqueInstance;
	}

}
```

하지만 이 방법은 여전히 동시성 문제가 남아있다. 서로 다른 두 쓰레드가 null 체크를 동시에 수행한 뒤 하나는 lock을 획득하여 new를 수행하고, 다른 쓰레드는 lock 획득을 기다린 후  new를 수행하게 될 수 있다.

이러한 문제를 해결하기 위해 double-check locking을 사용할 수 있다.
```java
public class Singleton {
	private static Singleton uniqueInstance;

	private Singleton() {}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized(Singleton.class) {
				if (uniqueInstance == null) {
					uniqueInstance = new Singleton();
				}
			}
		}
		return uniqueInstance;
	}

}
```

이 방법은 일단 lock을 획득해서 들어온 쓰레드들에게 uniqueInstance의 null 체크를 한번 더 수행하게 만든다. 이는 synchronized로 들어온 쓰레드들에게 한번 더 null 체크를 수행하게 만들어, 앞에서 제어를 가져갔던 쓰레드가 new를 호출했다면 다음번에 lock을 획득한 쓰레드들은 인스턴스 생성을 하지 않으므로, 논리적으로는 하나의 인스턴스가 유지된다.

이 방법은 최초의 if문이 없어도 논리적으로 하나의 인스턴스가 유지되긴 하지만, 최초 if문은 경쟁조건을 최소화하기 위한 목적으로 존재하므로 성능 향상의 이점이 있다.

하지만 이는 자바 언어의 특징으로 인해 멀티쓰레드에서 문제가 발생하곤 하였다. JVM 1.5 버전 이후에서는 volatile 키워드를 uniqueInstance 변수에 사용하여 메인 메모리의 주소를 직접 제어하고 레지스터나 로컬 메모리에 캐싱하지 않게 만들어 문제를 해결할 수 있다.

```java
public class Singleton {
	private static volatile Singleton uniqueInstance;

	private Singleton() {}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized(Singleton.class) {
				if (uniqueInstance == null) {
					uniqueInstance = new Singleton();
				}
			}
		}
		return uniqueInstance;
	}

}
```
