---
상위 개념: "[[Join Operation Processing]]"
---
# Block Nested-loop Join
Block Nested-loop Join은 Nested-loop Join의 확장이다. Nested-loop Join이 외부 루프는 레코드 단위로 수행되어 내부 릴레이션이 외부 릴레이션의 레코드 개수만큼 읽히는 문제가 있었다. 

Block Nested-loop Join은 이러한 문제를 방지하고자 내부 릴레이션의 모든 블록이 외부 릴레이션의 모든 블록과 쌍을 이루어 루프를 돌며, 각 블록 내의 레코드 쌍을 검사한다. 이 경우 내부 루프는 외부 릴레이션의 블록 개수만큼 접근된다.

## 알고리즘
```
for each block Br of R do begin
	for each block Bs of S do begin
		for each tuple tr in Br do begin
			for each tuple ts in Bs do begin
				test pair (tr, ts) to see if they satisfy the join condition
				if they do, add rs to the result
			end
		end
	end
end
```

## 비용

### 최악의 경우
만약 각 릴레이션 당 하나의 블록만 메모리를 차지할 수 있다면 비용은 다음과 같다. Nested-loop Join과 대조적으로 내부 릴레이션에 대한 전송 횟수를 $n_r$ 번이 아닌 $b_r$번 수행한다. Nested-loop Join은 내부 릴레이션에 루프를 돌 때마다 탐색을 수행했어야 하므로 $n_r$(내부 접근) + $b_r$(외부 접근) 이었지만, Block Nested-loop join에서는 내부접근이 $b_r$번 수행되어 $2*b_r$ 만큼 탐색이 발생한다. (내부 $b_r$, 외부 $b_r$)
* 전송 횟수 : $b_r * b_s + b_r$
* 탐색 횟수 : $2 * br$

### 최선의 경우
이 역시 Nested-loop Join과 마찬가지로 내부 릴레이션만 적재된다면 최선의 경우가 된다.
* 전송 : $b_r + b_s$
* 탐색 : 2회 (내부 릴레이션 접근 1회, 외부 릴레이션 접근 1회)