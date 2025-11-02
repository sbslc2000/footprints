---
상위 개념: "[[Pipe]]"
---
# Schema-based validation
Zod 라이브러리와 Custom Pipe를 함께 사용하여 DTO가 유효한 구조를 갖는지 검증할 수 있다.

다음과 같은 DTO가 존재한다고 해보자.
```ts
// create-cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
```

Zod를 사용하여 검증하는 공용 파이프는 다음과 같이 구성할 수 있다.

```ts
import { PipeTransform, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { ZodSchema } from 'zod';

export class ZodValidationPipe implements PipeTransform {
  constructor(private schema: ZodSchema) {}

  transform(value: unknown, metadata: ArgumentMetadata) {
    try {
      return this.schema.parse(value);
    } catch {
      throw new BadRequestException('Validation failed');
    }
  }
}
```

컨트롤러에서는 다음과 같이 사용한다.

```ts
import { z } from 'zod';
export const createCatSchema = z.object({
  name: z.string(),
  age: z.number(),
  breed: z.string(),
});

@Post()
@UsePipes(new ZodValidationPipe(createCatSchema))
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```