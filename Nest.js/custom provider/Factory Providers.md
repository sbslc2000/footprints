---
상위 개념: "[[Custom Provider]]"
---
# Factory Provider
Factory Provider는 `useFactory`를 사용한다. 이는 복잡한 초기화 로직이 필요할 때 유용하다.

팩토리 함수는 필요에 따라 다른 프로바이더를 의존성으로 받을 수 있으며, 이를 위해 inject 속성에 해당 프로바이더 목록을 명시한다. inject 배열에 명시된 순서대로 팩토리 함수의 파라미터로 전달된다.

```ts
const connectionProvider = {
  provide: 'CONNECTION',
  useFactory: (optionsProvider: MyOptionsProvider, optionalProvider?: string) => {
    const options = optionsProvider.get();
    return new DatabaseConnection(options);
  },
  inject: [MyOptionsProvider, { token: 'SomeOptionalProvider', optional: true }],
};

@Module({
  providers: [
    connectionProvider,
    MyOptionsProvider,
    // { provide: 'SomeOptionalProvider', useValue: 'anything' },
  ],
})
export class AppModule {}
```