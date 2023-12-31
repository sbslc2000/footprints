---
상위 개념: "[[String Matching Algorithm]]"
---
# Naive String Matching
![](https://i.imgur.com/Gkvd7fw.png)

원시적 매칭 방법은 마치 sliding window 처럼 한쪽에서부터 인덱스를 올려가며 문자열이 같은지 확인하는 알고리즘이다.

## Pseudo Code

```c
naiveMatching (A, P)
{
	D n: 배열 A()의 길이, m: 배열 P( )의 길이
	for is 1 to n-m+1 {
		if (P(1...m) = A[i...i+m-1])
			then A[i] 자리에서 매칭이 발견되었음을 알린다;
	}
}
```

for 루프는 n-m+1 회 반복되고, if문은 패턴의 길이만큼의 비교를 하기 때문에 O(m)의 시간이 든다. 따라서 전체 수행시간은 O(nm)이다. (m^2는 n에 비해 작으므로 상수취급한다.)

## 단점
원시적인 알고리즘을 사용했을 때 아래의 상황은 매우 비효율적이다.
![](https://i.imgur.com/3Yoc4Cx.png)

“abcdabcwz”의 문자열을 찾는다고 할 때, abcdabc”d” 에서 불일치가 발생했다. 이때 뒤에 있는 d가 불일치하므로 그 다음 인덱스를 기준으로 검사했을 때는 확실히 불일치 할 것이다. 그 다음 인덱스도 마찬가지이다. 원시적인 알고리즘은 이 인덱스들을 전부 검사 대상으로 한다. (=비교 과정의 문맥을 전혀 이용하지 않는다) 이 다음에 나오는 알고리즘들은 이러한 불필요한 검사를 어떻게 건너뛸지에 대해 서로 다른 방법으로 설계한 것들이다.