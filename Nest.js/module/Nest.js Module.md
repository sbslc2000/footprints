# Module
Nest.js의 Module은 애플리케이션을 기능 단위로 모듈화하고, 각 모듈 간의 의존성과 책임을 명확하게 분리하는 구조이다.. Nest의 모든 애플리케이션은 최소한 하나의 루트 모듈(AppModule) 을 가지고 있고, 그 안에 기능별 하위 모듈들이 포함되어 구조화된다.

```typescript
@Module({
  imports: [],         // 다른 모듈 임포트
  controllers: [],     // HTTP 요청을 처리하는 컨트롤러
  providers: [],       // 서비스, 리포지토리 등 의존성 주입 대상
  exports: [],         // 다른 모듈에서 사용할 수 있도록 내보내는 provider
})
export class SomeModule {}
```


## providers
Provider란 의존성 주입이 되는 대상 객체를 의미한다. 각 모듈의 프로바이더들은 `providers` 배열에 작성되어야 한다.

## exports
providers에 등록된 객체 중 모듈 외부로 노출하고자 하는 객체들을 등록한다.