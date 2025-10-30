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
