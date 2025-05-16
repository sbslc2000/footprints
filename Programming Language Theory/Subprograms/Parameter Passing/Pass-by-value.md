---
상위 링크: "[[Parameter Passing/Parameter Passing Methods]]"
---
# Call-by-value
Call-by-value는 In Mode 모델의 파라미터 전달 방식으로, 실 매개변수의 값이 형식 매개변수의 값을 초기화하는 방식으로 동작한다. 아래 코드로 예를 들자면, myPrint()로 전달된 a의 값이 호출된 서브프로그램에서 형식 매개변수인 f의 초기값으로 사용된다. 이후 f는 서브프로그램의 지역변수처럼 동작한다.
```c
int main()
{
	int a = 3;
	myPrint(a); 
}

int pow(int f) 
{
	return f * f;
}
```
이는 값이 물리적으로 복사되는 방식으로 구현되므로, f의 변경은 a에 영향을 주지 않는다.

이는 데이터의 전달이 오직 실 매개변수에서 형식 매개변수 방향으로만 이동하므로 In Mode에 해당한다.
