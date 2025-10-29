---
상위 개념: "[[Custom Provider]]"
---
# Class Provider
Class Provider는 환경에 따라 클래스 구현체를 동적으로 주입할 때 주로 사용된다.

```ts
const configServiceProvider = {
  provide: ConfigService,
  useClass:
    process.env.NODE_ENV === 'development'
      ? DevelopmentConfigService
      : ProductionConfigService,
};

@Module({
  providers: [configServiceProvider],
})
export class AppModule {}
```

이 경우 ConfigService 라는 토큰을 사용하는 클래스는 NODE_ENV의 값에 따라 달라지는 객체를 주입바을 수 있다.