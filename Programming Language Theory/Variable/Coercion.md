---
상위 링크: "[[Names, Bindings, Type Checking, and Scope]]"
---
# Coercion
> Automatic Type Conversion by compiler

 c언어에서 int와 float은 + 연산을 수행할 수 있으며, 이는 컴파일러에 의해서 int type이 float 타입으로 변환됨으로써 가능하다. 이러한 타입 자동 변환을 **Coercion**이라고 부른다.
 
```c
int main() {
	int a = 10;
	double b = 20.0, c;
	c = a + b;
}
```