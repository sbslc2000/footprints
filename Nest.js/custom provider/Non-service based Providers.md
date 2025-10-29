---
상위 개념: "[[Custom Provider]]"
---
# Non-service based Providers
프로바이더는 꼭 서비스 클래스일 필요가 없다. 단순한 값이나 배열을 반환할 수도 있다.

```ts
const configFactory = {
  provide: 'CONFIG',
  useFactory: () => {
    return process.env.NODE_ENV === 'development'
      ? devConfig
      : prodConfig;
  },
};

@Module({
  providers: [configFactory],
})
export class AppModule {}
```