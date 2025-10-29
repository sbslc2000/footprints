---
상위 개념: "[[Custom Provider]]"
---
# Alias Provider
Alias Provider는 `useExisting`을 사용하며, 기존 프로바이더에 별칭을 만들 수 있다. 이 방식은 동일한 인스턴스를 여러 이름으로 공유하고 싶을 때 유용하다.

```ts
@Injectable()
class LoggerService {
  /* 구현 내용 */
}

const loggerAliasProvider = {
  provide: 'AliasedLoggerService',
  useExisting: LoggerService,
};

@Module({
  providers: [LoggerService, loggerAliasProvider],
})
export class AppModule {}
```

위 등록으로 인해 'AliasedLoggerService'라는 이름으로도 LoggserService의 구현체를 주입받을 수 있다.