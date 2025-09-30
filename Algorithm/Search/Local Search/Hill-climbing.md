---
상위 개념: "[[Local Search]]"
---
# Hill-climbing
Hill-Climbing(HC) 혹은 Gradient descent라고 부르는 이 알고리즘은 현재 상태에서 더 나은 해를 주는 이웃 상태로 이동을 반복하는 [Local Search](Local%20Search.md) 알고리즘이다.

## 동작 방식
1. 탐색 공간 내의 해 중 최초 솔루션을 결정한다. 이를 현재 솔루션으로 간주한다.
2. 현재 솔루션으로부터 새로운 솔루션을 만들어내고(transformation), 이를 평가한다.
3. 만약 새로운 솔루션이 더 낫다면, 해당 솔루션을 현재 솔루션으로 간주하고, 아니라면 폐기한다.
4. 2번과 3번을 반복한다.
## Pseudo code
```
current <- guess
loop do
	neighbor <-- transform(current)
	if (eval(neighbor) is better than eval(current))
		current <- neighbor
end
```

## 장점과 단점
HC는 단순화고 직관적이며, 메모리 사용이 적다는 특징이 있다.

하지만, 지역 최적해(Local Optimum) 문제에 빠질 수 있다. 올바른 방향으로 가지 못하고 특정 봉우리에 갇힐 수 있다는 것이다.

이를 해결하기 위해서는 다음과 같은 방법론을 도입할 수 있다.

1. random restart
랜덤한 시작점에서 여러번 수행한다.

2. probabilistic approach
솔루션 선택에 확률적 방식을 도입한다.

3. increase the size of neighbors
변환 과정에서 이웃의 수를 늘린다.