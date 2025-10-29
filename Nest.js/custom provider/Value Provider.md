---
상위 개념: "[[Custom Provider]]"
---
# Value Provider
Value Provider는 상수 값이나 외부 라이브러리를 Nest 컨테이너에 주입할 때 사용한다. useValue는 값을 직접 요구하며, 이는 클래스 인스턴스나 리터럴 객체 모두 가능하다.

```ts
const mockCatsService = {
  /* mock 구현 */
};

@Module({
  imports: [CatsModule],
  providers: [
    {
      provide: CatsService,
      useValue: mockCatsService,
    },
  ],
})
export class AppModule {}
```
이 경우 CatsService 토큰은 mockCatsService 객체를 반환한다. 

> [!warning] 타입 불일치 문제
> useValue로 제공된 객체가 provide 토큰의 인터페이스를 구현하지 않는 경우가 발생할 수 있다. 이는 컴파일 타임에 오류로 잡히지 않으며, 런타임에 오류가 발생할 수 있으니 주의해야한다.

토큰은 클래스 뿐만 아니라 문자열, 심볼, enum 등으로 지정할 수도 있다.
```ts
@Module({
  providers: [
    {
      provide: 'CONNECTION',
      useValue: connection,
    },
  ],
})
export class AppModule {}
```
이 경우 'CONNECTION'이라는 문자열 토큰을 통해 connection 객체를 가져올 수 있다.

```ts
@Injectable()
export class CatsRepository {
  constructor(@Inject('CONNECTION') connection: Connection) {}
}
```
