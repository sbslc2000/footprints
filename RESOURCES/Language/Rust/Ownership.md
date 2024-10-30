---
상위 링크: "[[Rust]]"
공식 링크: https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html
---
# Ownership

> 소유권은 각 값에 대해 해당 값을 관리하는 고유한 소유자를 배정합니다. 그리고 컴파일러가 컴파일 타임에 소유권을 검증해 메모리 안전성을 보장합니다. 값은 소유자를 통해 대여할 수 있으며 이를 빌림(borrow)이라고 합니다. 빌림 기능을 통해 하나의 소유권에 대해 여러 개의 참조자를 만들 수 있습니다. 빌림을 사용하더라도 러스트의 컴파일러는 엄격한 소유권 규칙을 적용해 메모리 문제를 미연에 방지합니다.
- 알라딘 eBook <실전! 러스트로 배우는 리눅스 커널 프로그래밍> (김백기.우충기 지음) 중에서

소유권은 Rust 언어만의 특징이며, 메모리 문제를 해결하는 솔루션으로 작동한다.

## 소유권 이전 예시

``` Rust 
fn main() {
	let s1 = String::from("Hello, Ownership!");
	let s2 = s1;
	println("{}", s1); // Compile Error
}
```

위 러스트 코드는 컴파일 에러가 발생하는데, 이는 문자열 값이 s1에서 s2로 이동했기 때문이다. s1은 문자열 값에 대한 소유권이 없으므로 사용될 수 없다.

```Rust
fn main() {
	let s = String::from("Hello, Ownership!");
	push_str(s);
	println("{}", s); // Compile Error
}
```
소유권이 이전되는 상황은 단순히 다른 변수에 대입하는 행위 뿐 만 아니라 파라미터 전달로도 이루어진다. 위 경우 s의 소유권은 push_str 함수로 이전되었으므로 main의 이후 문장들에서 s에 접근할 수 없다.

## 빌림 Borrow
소유권을 완전히 이전하지 않고 참조를 전달하고 싶을 때는 borrow를 사용할 수 있다.

```Rust
fn main() {
	let mut s = String::from("Hello, Borrow");

	// s의 소유권을 push_str에 대여
	push_str(&mut s);

	// 소유권 보유중이므로 정상 수행
	println("{}",s);
}

fn push_str(s: &mut String) {
	s.push_str(" Hello");
}
```