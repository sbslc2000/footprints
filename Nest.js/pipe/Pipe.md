---
상위 링크: "[[../Nest.js]]"
---
# Pipe
![](https://i.imgur.com/wwLUcPQ.png)

Nest.js의 Pipe는 `PipeTransform` 인터페이스를 구현한 클래스로, @Injectable() 데코레이터와 함께 사용된다.

Pipe는 두 가지 목적으로 사용된다.
* 변환(transformation): 입력 데이터를 원하는 형태로 변형한다. (string -> integer) 
* 검증(validation): 입력 데이터가 유효한지 평가하여 통과시킬지 예외를 발생시킬지 결정한다.

두 경우 모두 컨트롤러의 메서드로부터 들어오는 인자에 대해 작동한다. Nest는 파이프를 컨트롤러 메서드가 수행되기 직전에 위치시켜 변환 또는 검증을 수행하고, 변환된 인자를 라우트 핸들러에 전달한다.

Nest는 미리 만들어진 여러개의 파이프(Built-in pipes)를 제공한다. 또한 사용자 정의 파이프를 만들 수도 있다.

> [!tip]
> Pipe는 예외 영역(Exception zone) 내에서 수행되기 때문에, 파이프에서 던져진 예외는 필터에 잡힌다.

## Built-in Pipes
Nest는 다음과 같은 내장 파이프를 제공한다. 이들은 모두 @nestjs/common 패키지에 존재한다.

- `ValidationPipe`
- `ParseIntPipe`
- `ParseFloatPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `ParseEnumPipe`
- `DefaultValuePipe`
- `ParseFilePipe`
- `ParseDatePipe`

예를 들어 ParseIntPite는 문자열을 정수로 변환하고, 변환에 실패하는 경우 예외를 던진다.

```ts
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

위 코드는 id가 정수로 변환될 수 없는 경우, 컨트롤러 메서드가 호출되기 전에 예외를 발생시킨다.

```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

## Custom Pipe
```ts
// validation.pipe.ts
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

PipeTransform<T, R> 인터페이스는 transform() 메서드를 가지고 있으며, 이를 구현하여 커스텀 파이프를 구현할 수 있다. value는 현재 처리 중인 인자이며, metadata는 인자에 대한 메타데이터가 들어있다.

```ts
export interface ArgumentMetadata {
  type: 'body' | 'query' | 'param' | 'custom';
  metatype?: Type<unknown>;
  data?: string;
}
```

## 예제: 기본 값 제공하기
Parse* 파이프는 기본적으로 null 또는 undefined 값을 허용하지 않는다. 따라서 쿼리 스트링 파라미터에 기본값을 제공하려면 DefaultValuePipe를 함께 사용해야한다.
```ts
@Get()
async findAll(
  @Query('activeOnly', new DefaultValuePipe(false), ParseBoolPipe) activeOnly: boolean,
  @Query('page', new DefaultValuePipe(0), ParseIntPipe) page: number,
) {
  return this.catsService.findAll({ activeOnly, page });
}
```