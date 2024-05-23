---
상위 링크: "[[NestJS]]"
---
# Pipe
![](https://i.imgur.com/wwLUcPQ.png)
Nest.js의 Pipe는 PipeTransform 인터페이스를 구현한 클래스로, @Injectable() 데코레이터와 함께 사용된다.

Pipe는 두 가지 목적으로 사용된다.
* **transformation**: 입력 데이터를 원하는 형태로 변형한다. (string -> integer) 
* **validation**: 입력 데이터가 유효한지 평가하여 통과시킬지 예외를 발생시킬지 결정한다.

두 경우 모두 Controller Route Handler로부터 들어오는 argument에 대한 처리를 수행한다. Nest는 pipe를 컨트롤러 메서드가 수행되기 직전에 위치시켜 제어한 뒤 컨트롤러 메서드가 수행되게 만든다.

Nest는 미리 만들어진 여러개의 Pipe를 제공한다. 또한 Custom Pipe를 만드는 기능도 제공한다.

Pipe는 Exception zone 내에서 수행되기 때문에, Pipe에서 던져진 예외는 Exception Filter에 잡힌다.

## Built-in Pipes
Nest는 다음 9개의 Pipe를 제공한다. 이들은 모두 @nestjs/common 패키지에 존재한다.
- ValidationPipe
- ParseIntPipe
- ParseFloatPipe
- ParseBoolPipe
- ParseArrayPipe
- ParseUUIDPipe
- ParseEnumPipe
- DefaultValuePipe
- ParseFilePipe

## 사용법
파이프를 사용하기 위해서는 파이프 클래스를 적당한 위치에 바인딩해야한다. ParseIntPipe를 예로 들자면, Controller의 메서드에서 받아야하는 input이 존재하는 곳에 다음과 같이 삽입하여 해당 타입의 런타임 값이 number 타입임을 보장할 수 있다.

```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

