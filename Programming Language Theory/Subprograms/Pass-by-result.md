---
상위 링크: "[[Parameter Passing Methods]]"
---
# Pass-by-result
Pass-by-result는 Out Mode에 해당하는 파라미터 전달 방식이다.

실 매개변수의 값은 서브프로그램으로 전달되지 않으며, 수행이 끝나고 제어가 호출자에게 넘어가기 전에 형식 매개변수의 값이 실 매개변수로 전달된다.
```c
int sub(int x, int y) 
{
	x = 10;
	y = 3;
	return x - y; //제어가 끝나기 전, x 와 y에 해당하는 값이 실 매개변수로 넘어간다
}

int main()
{
	int a = 0, b = 0, c = 0;
	c = sub(a, b); // a와 b의 값인 0은 서브프로그램으로 전달되지 않는다.
	printf("%d, %d, %d\n", a, b, c); // 10, 3, 7
}
```

