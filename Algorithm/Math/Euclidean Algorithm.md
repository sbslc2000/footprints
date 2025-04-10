---
상위 개념: "[[Algorithm]]"
---
# Euclidean Algorithm
유클리드 호제법*Euclidean Algorithm*은 두 정수의 최대공약수를 쉽게 알아내는 방법이다.

## Process
A와 B의 최대공약수 GCD(A,B)를 알아내는 유클리드 호제법은 다음과 같다.

* A = 0 이라면, GCD(A, B) = B이다.
* B = 0 이라면, GCD(A, B) = A이다.
* A를 A = B * Q + R의 형식으로 쓴다.
* GCD(A,B) = GCD(B,R) 이므로, 다시 유클리드 호제법을 이용하여 GCD(B,R)을 찾는다.

## 예시 
270과 192의 최대공약수를 찾는 로직은 다음과 같다.

1. 270 = 192 * 1 + 78 이다.
2. GCD(270,192) = GCD(192, 78) 이다.

3. 192 = 78 * 2 + 36 이다.
4. GCD(192, 78) = GCD(78, 36) 이다.

5. 78 = 36 * 2 + 6 이다.
6. GCD(78,36) = GCD(36, 6)이다.

7. 36 = 6 * 6 + 0 이다.
8. GCD(36, 6) = GCD(6, 0)이다.

9. GCD(6,0)은 6이다.
10. 따라서 GCD(270, 192)는 6이다.


## 예제 코드
```java
public int gcd(int a, int b) {
	int temp;
	while (b != 0) {
		temp = b;
		b = a % b;
		a = temp;
	}

	return a;
}
```