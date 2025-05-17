---
상위 링크: "[[Names, Bindings, Type Checking, and Scope]]"
---
# Scope
변수의 Scope란 해당 변수를 볼 수 있는(visible) 문장들의 범위이다. 여기서 볼 수 있다는 뜻은 해당 문장에서 변수가 참조 가능하다는 것이다.

```c
int a;

int main(void) {
	int b;
	--(1)
}

void func(int c) {
	--(2)
}
```

전역변수인 a는 1과 2 지점에서 모두 접근할 수 있으므로, a는 이 모든 지역을 스코프로 갖는다. 하지만 지역변수인 b는 main 블록이라는 제한된 스코프를 갖는다.

## Static Scoping

## Dynamic Scoping
>