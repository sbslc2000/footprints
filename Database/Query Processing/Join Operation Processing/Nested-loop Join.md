---
상위 개념: "[[Join Operation Processing]]"
---
# Nested-loop Join
중첩 루프 조인은 중첩 for 문 알고리즘을 통해 조인 연산을 처리한다. 이 때 바깥쪽 중첩문에 해당하는 릴레이션을 외부 릴레이션*outer relation*, 안쪽 반복문에 해당하는 릴레이션을 내부 릴레이션*inner relation*이라고 한다. 

## 알고리즘
```
// theta R to S 
for each tuple r in R
	for each tuple s in S
		test pair (r, s) to see if they satisfy the join condition
		if they do, add rs to the result
	end
end
```
릴레이션 R의 각각의 튜플 r에 대해서, 릴레이션 S의 각각의 튜플 s와 조인 조건에 맞는지 검사하여 맞다면 결과 셋에 추가하고, 아니라면 다음 조사를 이어간다.
