# Exception Filter
Nest.js에는 애플리케이션에서 발생했지만 사용자의 코드로부터 처리되지 않은 예외들을 처리하는 내장된 기본 예외 처리 레이어가 존재한다. 이는 Global Exception Filter에 의해 수행되며, HttpException과 그 하위 예외들을 처리하여 적절한 응답을 만들고, HttpExcpetion이 아닌 예외들에 대해서는 다음 응답을 반환한다.
```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

## Throwing Standard Exceptions
Nest는 `@nestjs/common` 패키지에 내장된 HttpException 클래스를 제공한다. 

HttpException의 생성자는 두 개의 필수 파라미터를 받는다.
1. response : JSON 응답 본문 (문자열 혹은 객체)
2. status : HTTP 상태 코드

```ts
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

> [!tip] HttpStatus
> `@nestjs/common` 에는 HTTP 상태들을 enum화 한 HttpStatus를 제공한다.

위 경우 클라이언트는 다음과 같은 응답을 받는다.
```json
{
  "statusCode": 403,
  "message": "Forbidden"
}
```

만약 response에 객체를 지정했다면, 응답 본문 전체가 지정한 객체로 재정의된다.

```ts
@Get()
async findAll() {
  try {
    await this.service.findAll();
  } catch (error) {
    throw new HttpException(
      {
        status: HttpStatus.FORBIDDEN,
        error: 'This is a custom message',
      },
      HttpStatus.FORBIDDEN,
      { cause: error },
    );
  }
}
```

세 번째 인자로 options를 제공할 수 있는데, options의 기능 중 하나는 에러 원인(cause)를 추가하는 것이다. 에러 원인은 응답으로 반환되지 않지만 로깅되므로 디버깅에 도움이 된다.

## Exception Logging
기본적으로 Nest의 예외 필터는 HttpException 같은 내장 예외를 콘솔에 로깅하지 않는다. WsException, RpcException도 동일하다. 이 예외들은 모두 @nestjs/common의 `IntrinsicException` 클래스를 상속하며, 이를 통해 시스템은 '정상적인 예외'와 '비정상적인 예외'를 구분할 수 있다.

따라서 예외를 직접 로깅하고 싶다면, 커스텀 예외 필터를 생성해야한다.

## Custom Exceptions
대부분의 경우 기본 HttpException을 사용하면 충분하나, 맞춤형 예외 계층을 만들고 싶다면 HttpException을 상속받아 구현할 수 있다.

```ts
// forbidden.exception.ts
export class ForbiddenException extends HttpException {
  constructor() {
    super('Forbidden', HttpStatus.FORBIDDEN);
  }
}
```

```ts
@Get()
async findAll() {
  throw new ForbiddenException();
}
```

## 예외 필터
로깅을 추가하거나 JSON 응답 형식을 동적으로 변경해야할 때 예외 필터를 만들 수 있다.

```ts
// http-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
} from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
    });
  }
}
```

예외 필터는 반드시 `ExceptionFilter<T>` 인터페이스를 구현해야 한다. 

`@Catch()` 데코레이터를 통해 이 필터가 어떠한 예외 타입을 처리할지 지정할 수 있다. 만약 여러 예외를 처리하게 만들고 싶다면 `@Catch(A, B, C)` 처럼 여러 타입을 지정할 수 있다. 아무것도 명시하지 않는다면 모든 예외를 처리하게 된다.

### Binding Filters
필터는 메서드 단위, 컨트롤러 단위, 전역 단위로 바인딩할 수 있다.

1. 특정한 라우트에 적용
```ts
@Post()
@UseFilters(new HttpExceptionFilter())
async create(@Body() dto: CreateCatDto) {
  throw new ForbiddenException();
}
```

2. 컨트롤러 전체에 적용
```ts
@Controller()
@UseFilters(new HttpExceptionFilter())
export class CatsController {}
```

3. 전역 적용
```ts
// main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter());
  await app.listen(3000);
}
```

### 의존성 주입
전역 필터가 의존성 주입을 필요로 한다면, 다음과 같이 `APP_FILTER`를 이용해 모듈 내에서 등록할 수 있다.
```ts
@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}
```