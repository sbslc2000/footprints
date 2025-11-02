---
상위 개념: "[[../Nest.js|Nest.js]]"
---
# Guard
가드는 `@Injectable` 데코레이터로 주석이 달린 클래스이며, `CanActivate` 인터페이스를 구현한다.

가드는 특정 조건 (권한, 역할, ACL 등)에 따라 들어온 요청을 라우트 핸들러에서 처리할 지 여부를 결정한다. 이런 처리는 보통 인가(Authorization)라고 불린다.

일반적으로 인증(Authentication)은 전통적인 Express 애플리케이션에서는 보통 미들웨어(Middleware)로 처리되었다. 토큰 검증이나 요청 객체에 사용자 정보를 추가하는 등의 작업은 특정한 라우트 컨텍스트와 결합되지 않기 때문이다.

하지만 미들웨어는 '어떤 핸들러가 실행될지' 알 수 없다. 인가 작업은 이러한 맥락정보를 필요로 한다. Guard는 ExecutionContext 인스턴스에 접근할 수 있으므로, 어떤 핸들러가 실행될 지 정확히 알고 있다. 이러한 이유로 Guard는 예외 필터, 파이프, 인터셉터와 유사하게 요청/응답 사이클에서 원하는 시점에 선언적으로 로직을 개입시킬 수 있어 인가 작업을 처리하는데에 적합하다.

모든 가드는 반드시 `canActivate()` 메서드를 구현해야 한다. 이 메서드는 현재 요청을 허용할 지 여부를 결정하는 boolean 값을 반환해야 한다. 이 값은 동기 또는 비동기로 반환할 수 있다. true인 경우 요청을 계속 진행하고, false인 경우 요청을 거부한다.

> [!info] 가드의 실행 시점
> 가드는 모든 미들웨어가 실행된 이후, 인터셉터나 파이프가 실행되기 이전에 동작한다.

## ExecutionContext
`canActivate()`는 ExecutionContext 객체 단 하나만을 인자로 받는다. 

이 객체를 사용하면 요청(Request) 객체에 접근하거나, 실행 중인 핸들러의 메타데이터를 조회할 수 있다.

## Binding Guards
가드 역시 적용 범위를 지정할 수 있다.

1. 컨트롤러 범위
```ts
@Controller('cats')
@UseGuards(RolesGuard)
export class CatsController {}
```
2. 메서드 범위
메서드 위에 달아주면 된다.

3. 전역 범위
```ts
const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new RolesGuard());
```

이 방법은 RolesGuard를 의존성 주입하기에는 적합하지 않다. 이 경우에는 APP_GUARD 토큰을 통해 등록하면 된다.
```ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: RolesGuard,
    },
  ],
})
export class AppModule {}
```


## 대표 사례 : Authorization Guard
위에서 언급했다싶이 인가는 가드의 대표적인 활용 사례이다. 다음 예시의 `AuthGuard`는 인증된 사용자가 있다고 가정하며, 요청 헤더에 토큰이 포함되어 있다고 가정한다. 가드는 이 토큰을 추출하여 검증하고, 검증 결과에 따라 요청을 허용할지 거부할지를 결정한다.
```ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext):
    boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

