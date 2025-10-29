---
상위 개념: "[[../Nest.js|Nest.js]]"
---
# Custom Provider
Custom Provider는 Nest.js의 IoC 컨테이너에 컴포넌트를 등록하는 다양한 방식을 다룬다.

# Standard Provider

```ts
@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
```

통상적으로 위와 같이 providers 배열에 Nest 컨테이너에 등록될 컴포넌트 클래스를 작성하는데, 이는 사실 아래 코드의 단축 문법이다.

```ts
providers: [
  {
    provide: CatsService,
    useClass: CatsService,
  },
];
```

이는 CatsService라는 토큰을 CatsService 클래스와 연결한다는 의미이다.

이러한 표준 프로바이더 이상의 요구사항이 있을 때에는 어떻게 해야할까?

* Nest가 인스턴스를 직접 생성하지 않고, 사용자 정의 인스턴스를 사용하고 싶을 때
* 기존 클래스의 인스턴스를 다른 의존성에서 재사용하고 싶을 때
* 테스트 시 Mock 객체로 교체하고 싶을 때

Nest.js는 이러한 상황을 위해 여러 형태의 커스텀 프로바이더를 제공한다.