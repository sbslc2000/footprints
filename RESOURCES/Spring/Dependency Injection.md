# Dependency Injection

Dependency Injection에서 다루는 개념들을 소개합니다.

## DIP (Dependency Inversion Principle)

DIP 는 ‘**의존성 역전 원칙**’으로, 객체지향의 5원칙 SOLI**D** 중 마지막 원칙에 해당하는 내용입니다.
의존의 역전이라는 것이 어떤 의미인지 먼저 알아보겠습니다. 기존의 소프트웨어는 **TOP-DOWN** 형식으로 의존성을 가졌습니다.

예를 들어 Controller 계층은 Service 계층을 의존하고, Service 계층은 Repository 계층을 의존하고, Repository는 더 하위 레벨의 Database Module 을 의존하고, 이것은 네트워크 모듈을 의존하고.. 이렇게 하위레벨로 점점 내려가는 의존을 TOP-DOWN 형식의 의존성이라고 합니다.

[![](https://blog.kakaocdn.net/dn/b2N2pK/btsAzDvdfLO/KSEmO27l0EXWuiTTFECVKK/img.png)](https://blog.kakaocdn.net/dn/b2N2pK/btsAzDvdfLO/KSEmO27l0EXWuiTTFECVKK/img.png)

```
class LowModule{

	void hello {
		print("hello");
	}
}

class HighModule {
	private LowModule lowModule = new LowModule();

	public someMethod() {
		lowModule.hello();
	}
}
```

DIP 원칙은 이렇게 아래 화살표로 내려가는 의존성을 반대로, 아래에서 위로(BOTTOM-UP) 올려야 한다는 의미입니다. 이 원칙을 지키기 위해서는 두 모듈 사이에 추상화가 필요합니다.

[![](https://blog.kakaocdn.net/dn/olJ2Q/btsAw7qmaLP/Z6LDMilnDjxb9Mm1ctOTk1/img.png)](https://blog.kakaocdn.net/dn/olJ2Q/btsAw7qmaLP/Z6LDMilnDjxb9Mm1ctOTk1/img.png)

이 추상화 계층을 Java에서는 **==Interface와 abstract class==**를 통해 사용합니다. 위 구조를 따른 프로그램에서 High-level Module은 Low-Level Module을 직접 사용하지 않고, 그것을 추상화한 인터페이스만을 갖고 있습니다. High-level module 은 Abstraction Layer를 의존합니다.

Low-level module은 Abstraction Layer 을 상속하여 그 내용들을 구현합니다. 따라서 Low-level Module은 Abstraction Layer 를 의존합니다.

기존에는 Top-down 형식으로 의존성의 화살표가 내려갔다면, DIP 구조에서는 Low-level Module이 상위 계층을 의존하여 의존성이 역전된 모습을 볼 수 있습니다.

코드로 예시를 든다면 다음과 같습니다.

```
interface AbstractLayer {
	void hello();
}

class LowModule implements AbstractLayer {
	@Override
	void hello {
		print("hello");
	}
}

class HighModule {
	private AbstractLayer abstractLayer;
}
```

여기서 HighModule을 보면 AbstractLayer 에 의존하고있습니다. 하지만 이 경우 인터페이스는 인스턴스화 할 수 없기 때문에 인스턴스화 할 수 있는 AbstractLayer를 구현한 객체가 필요합니다.

```
class HighModule {
	private AbstractLayer abstractLayer = new LowModule();

	public someMethod() {
		abstractLayer.hello();
	}
}
```

위 코드처럼 AbstractLayer를 구현한 구현체 클래스를 생성하여 구현체의 메서드를 사용할 수 있습니다. 하지만 위와 같은 구조는 결국 abstractLayer 역할을 하는 구현체를 변경할때 HighModule을 수정해줘야한다는 문제를 그대로 갖고 있습니다.

## Injector

결국 문제를 해결하기 위해서는 **제 3의 객체**가 필요합니다.

의존주입에서 제 3의 객체는 **Injector**라고 부릅니다.(혹은 다양한 이름으로 부르기도 합니다. ex.DI Container, Assembler, Factory, …) 이는 객체들의 의존관계를 주입 혹은 조립한다는 의미를 갖고 있습니다.

앞에서 봤던 문제들의 본질은 High Module에서 Low Module이라는 **구현체를 직접 생성**한다는데에서 발생하는 문제였습니다. High Module에서 Low Module을 직접 생성하기 때문에 그 정보를 ‘알아’야하고 따라서 구현체에 의존하는 강한 결합 관계가 발생합니다.

Assembler는 이러한 결합관계를 약한 결합관계로 만들고자 구현체들을 생성하고, 필요로 하는곳에 뿌려주는 역할을 합니다.

```
class Injector {
	private static AbstractLayer abstractLayer = new LowModule();
	
	static AbstractLayer injectAbstractLayer() {
		return abstractLayer;
	}
}

class HighModule {
	private AbstractLayer abstractLayer = Injector.injectAbstractLayer();

	public someMethod() {
		abstractLayer.hello(); //hello
	}
}
```

Injector를 사용한 예시입니다.

Injector는 AbstractLayer의 레퍼런스를 갖고있으며, 구체적인 구현체를 생성하여 갖고 있습니다. 이후 Injector에게 특정 인터페이스의 구현체를 주입해달라는 요청이 오면, 갖고있던 구현체를 반환해줍니다.

HighModule은 AbstractLayer의 역할을 하는 구현체가 필요합니다. 따라서 Injector의 injectAbstractLayer를 호출하여 어셈블러에게 구현체를 가져다달라고 합니다. 이후 LowModule의 레퍼런스를 주입받게되고, High Module은 Low Module을 인터페이스 레퍼런스를 통해 사용하게 됩니다.

==**변경된 HighModule의 코드에서는 어떠한 LowModule에 대한 정보도 찾을 수 없습니다**==. 이 뜻은 곧 LowModule과의 의존성이 제거됐음을 의미합니다. High Module은 AbstractLayer의 역할을 하는 구현체가 어떤 것인지 모르지만, 어떠한 역할을 하는 구현체가 주입됐음만 알고있고 그 객체와 메시지를 주고받으며 소통을 합니다.

이제는 지금까지 봤었던 100개의 클래스를 수정해야하는 문제에서 해방되었습니다. 만약 AbstractLayer의 역할을 수행해야하는 새로운 클래스를 만들고 그 클래스를 사용하고자 하는 곳에 적용시켜주려면 다음과 같이 하면 됩니다.

```
class NewLowModule implements AbstractLayer {
	@Override
	void hello() {
		System.out.println("HELLO!!!@#!@#!@#!@");
	}
}

class Injector {
	private AbstractLayer abstractLayer = new NewLowModule();
	
	static AbstractLayer injectAbstractLayer() {
		return abstractLayer;
	}
}

class HighModule {
	private AbstractLayer abstractLayer = Injector.injectAbstractLayer();

	public someMethod() {
		abstractLayer.hello(); //"HELLO!!!@#!@#!@#!@"
	}
}
```

AbstractLayer를 구현한 구현체 NewLowModule을 생성하고, Injector에게 앞으로 abstractLayer 를 요청하는 곳에 NewLowModule 을 반환해줄 것을 설정해줍니다.

결국 Injector의 코드 한 줄만 수정해주면, AbstractLayer를 사용하는 5조5억개의 클래스는 어떠한 코드를 변경하지 않아도 새롭게 바꿔준 클래스를 사용하게 됩니다.

이를 책임의 관점에서 생각한다면, **기존에는 High Modue에서 사용하는 객체를 “생성”하는 역할과 “사용”하는 역할을 둘 다 가졌다면, 지금은 “생성”하는 역할은 생성만 담당하는 객체(Injector)가 맡고 “사용”의 역할만 갖는다**고 이야기 할 수 있겠습니다.

## 의존 주입의 장점

이제는 상위 모듈이 하위 모듈과 약한 결합관계를 갖기 때문이 변경시 영향이 미미해집니다. 사용하는 객체가 바뀌어도 어떠한 코드의 변경도 필요하지 않습니다.

또한 이제 상위 모듈이 인터페이스를 의존하기 때문에 Mocking이 가능해집니다. Mocking 이란 가짜 객체를 주입해서 테스트하는 기법을 의미합니다.