---
상위 개념: "[[../Typescript|Typescript]]"
---
# Never 
Never은 절대 발생하지 않는 값을 나타낸다. 이 타입은 반환하지 않는 함수, 값이 할당되지 않는 변수, 항상 예외를 발생시키는 표현식 등을 나타낼 때 사용된다. 일반적으로 복구 불가능한 오류가 발생하거나, 프로그램의 흐름이 결코 해당 지점에 도달하지 않아야 하는 상황에서 활용된다.

## 사용처

### 절대 반환할 일이 없는 함수
```typescript
function throwError(message: string): never {
    throw new Error(message);
}

function infiniteLoop(): never {
    while (true) {
        // 무한 루프, 절대 반환되지 않음
    }
}
```
이 예제에서 함수들은 명시적으로 never 반환 타입을 선언하여, 정상적인 값을 절대 반환하지 않음을 나타낸다.

### 타입 가드
```typescript
function assertNever(value: never): never {
    throw new Error(`Unexpected value: ${value}`);
}

function processValue(value: string | number): void {
    if (typeof value === 'string') {
        // 문자열 처리
    } else if (typeof value === 'number') {
        // 숫자 처리
    } else {
        assertNever(value); // 도달 불가능한 코드: value가 다른 타입일 수 없음
    }
}
```
이 예제에서 assertNever 함수는 타입 가드로 사용되어 모든 가능한 타입이 처리되었는지를 보장한다.