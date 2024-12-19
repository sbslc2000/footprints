---
상위 링크: "[[Primitive Type]]"
---
# Floating-point
![m8mtt6Z.png](https://i.imgur.com/m8mtt6Z.png)
부동소수점은 실수를 표현하기 위한 한 방법이다. 부동소수점은 대부분의 실수 값의 근사값만을 저장할 수 있다. 

부동소수점은 가수*fraction*와 지수*exponent*로 구성된다. 가수는 실수의 유효숫자로, 수의 정밀도와 소수점 이하의 값을 나타낸다. 지수는 실수의 크기를 결정하며, 가수를 2의 거듭 제곱으로 곱하여 수의 크기를 조정한다.
$$f * 2^{e - bias}$$

IEEE 754에는 float type의 규격을 정의하고 있다. float은 4byte이며, 지수부가 8비트, 소수부가 23비트로 처리된다. 더 정밀하거나 큰 값이 요구된다면 지수부가 11비트, 소수부가 52비트인 double 타입을 사용할 수도 있다.

근사값만을 저장하기 때문에 $\pi$나 $e$과 같은 것을 정확하게 표현할 수 없다. 또한 0.1와 같은 값도 표현할 수 없다는 한계가 있다.

```
if (0.1 + 0.2 == 0.3)  //false
```