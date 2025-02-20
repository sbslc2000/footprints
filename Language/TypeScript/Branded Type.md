---
상위 링크: "[[Typescript]]"
---
# Branded Type

```typescript
type Brand<K, T> = K & { __brand: T };

export type UserID = Brand<string, 'UserID'>;  
export type OrderID = Brand<string, 'OrderID'>;
```

Branded Type은 변수의 용도를 명확하게 표현하고, 프로그램을 안정적이고 정확하게 동작할 수 있게 하며, 유지보수성을 높여준다.

## 예시

다음과 같은 클래스가 있다고 해보자.
```typescript
class User {
	private readonly id: string;
}
```

User를 가져오는 레포지토리 클래스는 다음과 같이 작성될 수 있다.
```typescript
class UserRepository {
	public async findById(userId: string): Promise<User> {...}
}
```

사용자 식별자 타입의 변경을 대비하려면 다음과 같은 방식으로도 작성할 수 있다.
```typescript
class UserRepository {
	public async findById(userId: User['id']): Promise<User> {...}
}
```

하지만 여기서 컴파일 타임의 userId는 string으로 다음과 같은 문제가 발생할 수도 있다.
```typescript
const userId: string = '1';
const orderId: string = '2';

const user = await userRepository.findById(orderId);
```

userId를 넣어야하는 곳에 실수로 orderId를 넣어버린 것이다! 뇌빼고 프로그래밍하면 충분히 발생할 수 있는 일이지만, 컴파일 에러를 잡아주지 않아 런타임에 가서야 문제를 인지하게 된다.

Branded Type은 이러한 문제를 방지하도록 도와준다.

```typescript
//types.ts
type Brand<K, T> = K & { __brand: T };
export type UserID = Brand<string, 'UserID'>;  

//User.ts
class User {
	private readonly id: UserID;
}

//UserRepository.ts
class UserRepository {
	public async findById(userId: UserID): Promise<User> {...}
}


```

User의 식별자에 해당하는 타입을 별도의 UserID라는 이름의 타입으로 작성해주는 것이다. Branded Type은 기본적으로는 기입한 타입처럼 동작한다.  허나 이제 UserID를 파라미터로 요구하는 메서드를 작성할 때는 단순히 string을 넘겨서는 컴파일에 통과하지 않고, \_\_brand 값의 동일성까지 검사하기 때문에, 기존에 발생했던 상황은 컴파일 에러에 잡히게 된다.

```typescript
const userId: string = '1' as UserID;
const orderId: string = '2' as OrderID;

const user = await userRepository.findById(orderId); // compilation error
```

