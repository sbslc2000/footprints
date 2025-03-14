---
상위 링크: "[[Primitive Type]]"
---
# Integer Type
Integer Type, 정수형 타입은 가장 흔한 원시 데이터 타입이며, 많은 컴퓨터들은 다양한 사이즈의 정수형 타입( byte, word, long word, quadword, ...) 을 지원한다.

C나 C++와 같은 언어는 부호가 없는 정수형을 지원하기도 하며, 이는 주로 비트 연산이나 이진 데이터를 다룰 때 사용되기도 한다.

## Implementation
정수 자료형을 구현할 때에는 비트 문자열로 표현한다. 2의 보수법을 사용한다면 가장 왼쪽에 있는 비트는 부호를 표현하며, 나머지 자릿수들로 값을 표현한다. 하지만 모든 정수형 타입이 이러한 구현을 내부적으로 가지며 하드웨어의 지원을 받는 것은 아니다. Python 의 Long Integer와 같은 타입은 정수형이지만 무한한 길이를 갖는 특성으로 인해 다르게 처리된다.