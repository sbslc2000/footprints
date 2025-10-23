---
상위 개념: "[[Hill-climbing]]"
---
# Stochastic Hill-Climbing
Stochastic Hill-Climbing이란 [HC](Hill-climbing.md)의 단점을 극복하기 위해서, 확률적 방법론을 도입한 방식이다. 이웃 솔루션에 대해서 비록 그것이 더 낮은 평가값을 가지고 있더라도, 확률적으로 그것을 채택하도록 만든다. 이는 더 넓은 공간을 탐색하게 만들어 지역 최적해에서 벗어날 가능성을 만든다.

```
proc stochastic hc
	t <- 0
	select a current Vc (random)
	evaluate Vc
	Repeat 
		create the new Vn from Vc (local transformation)
		select Vn with "Probability"
	
		t <- t + 1
	until t <- MAX
```