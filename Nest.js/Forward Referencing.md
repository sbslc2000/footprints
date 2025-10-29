---
상위 개념: "[[Nest.js]]"
---
# Forward Referencing
DI 컨테이너는 컴포넌트들을 생성하고, 그 관계성을 참고하여 필요로 하는 객체를 주입해주는 역할을 한다. 순환 의존성(Circular Dependency) 문제는 두 클래스가 서로를 의존할 때 발생한다. 클래스 A를 인스턴스화하는 과정에서 생성자를 호출할 때 클래스 B의 인스턴스를 요구한다고 하자. 이 때 B를 생성하는 과정에서도 A의 인스턴스를 요구하는 경우 순환성 문제가 발생한다.

순환 의존성 문제가 발생하는 것은 주로 올바른 객체지향 설계가 되지 않았을 때이다. 따라서 가능한 한 순환 의존성 문제를 피하는 방식으로 설계하는 것이 좋지만, Nest.js는 여러 방법을 통해 순환 의존성 문제를 해결하도록 만든다. 전방 참조(forward referencing)은 그 방법 중 하나이다.

`forwardRef()`는 아직 정의되지 않은 클래스를 참조할 수 있도록 해주는 Nest.js의 유틸리티 함수이다. 예를 들어 `CatsService`와 `CommonService`가 서로를 의존한다면(의존하지 않는 것이 최우선 해결책이겠지만) 두 클래스 모두에 @Inject()와 forwardRef()를 의용해 순환 의존성을 해결할 수 있다.

```ts
@Injectable()
export class CatsService {
  constructor(
    @Inject(forwardRef(() => CommonService))
    private commonService: CommonService,
  ) {}
}
```

```ts
@Injectable()
export class CommonService {
  constructor(
    @Inject(forwardRef(() => CatsService))
    private catsService: CatsService,
  ) {}
}

```

> [!warning] 인스턴스 생성 순서
> 인스턴스의 생성 순서는 정해져 있지 않다. 따라서 어떤 클래스의 생성자 호출 순서에 의존하는 프로그래밍을 해서는 안된다.
