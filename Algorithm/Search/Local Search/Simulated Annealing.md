---
상위 개념: "[[Stochastic Hill-Climbing]]"
---
# Simulated Anneling
```
Procedure SA
begin
	t <- 0
	T <- initial value
	select Vc
	evaluate Vc.
	Repeat
		Repeat
			select Vn <- using Vc (transformation)
			if (Vn is better than Vc), then Vc <- Vn
			else if (rand() < p) then Vc <- Vn
		Until(termination-condition)
		T <- g(T, t)
		t <- t + 1
	Until(termination-condition e.g., t = max)
end
```
Simulated Annealing(SA)는 고도화된 [Stochastic Hill-Climbing](Stochastic%20Hill-Climbing.md) 알고리즘이다. SA는 온도(temparature)라는 개념을 통해 초기에 나쁜 해도 확률적으로 수용하여 [Local Optima](Local%20Optima.md)에 갇히는 문제를 해결한다. 온도는 초기에 높은 값으로 설정되어 나쁜 해를 적극적으로 수용하지만, 프로시저가 동작함에 따라 점차 줄어들어 최적해를 찾는데에 집중한다.

SA는 이웃들에 대해 다음과 같은 규칙을 통해 현재 솔루션으로 선택할지 말지를 결정한다.

1. 만약 이웃이 현재 해보다 더 좋다면, 해당 이웃을 현재 해로 받아들인다.
2. 만약 이웃이 현재 해 보다 더 나쁘다면, 확률에 의해 그것을 받아들인다.

받아들일 확률을 결정하는 수식은 다음으로, 두 요소에 따라 결정된다.
$$ P = \frac{1}{1 + e^{\frac{eval(V_c) - eval(V_n)}{T}}}$$
1. 상수 T (temparature)
2. 해의 평가 값 차이 (eval($V_c$) - eval($V_n$))

## Temparature
최대화 문제를 가정한다. eval($V_c$)가 120이고 eval($V_n$)이 107이라고 해보자. 이웃의 평가값이 더 낮은 경우이다. 위 수식에 따라 상수 T는 다음과 같은 효과를 갖는다.

1. 만약 T가 1이라면, P는 0.00을 결과로 갖게 되고, 해당 솔루션은 버려질 것이다.
$$P = \frac{1}{1 + e^{\frac{-13}{1}}} = 0.00$$
2. 만약 T가 10이라면, P는 0.21을 갖게 되어 약 20%의 확률로 받아들여질 것이다.

3. 만약 T가 $\infty$라면, P는 0.5가 되고, 해당 솔루션은 랜덤하게 받아들여진다.
$$P = \frac{1}{1 + e^{\frac{13}{\infty}}} = \frac{1}{1 + e^0} = \frac{1}{2} = 0.5$$


즉 T가 높으면 높을수록, 해의 평가 값 차이는 의미를 잃고 그저 운에 의해 다음 솔루션이 결정된다. 만약 T가 낮으면 낮을 수록, 운의 영역은 사라지고 해의 평가 값 차이가 결과를 지배하게 된다.

> The greater the value T, the smaller the impact $V_c - V_n$

결국 T의 값이 높다면 나쁜 솔루션이라도 채택할 확률이 높아진다는 것이다. 

> [!info] 
> T가 0인 경우, 이는 일반적인 HC와 동일하게 동작한다. 즉, 확률적 요소는 전부 없어지고 오직 평가값에 의해 이웃을 선택할지 말지가 결정된다.


## 평가 값 차이
온도가 동일한 경우, 해당 해를 받아들일지 아닐지는 전적으로 평가 값의 차이에 달려있다. eval($V_c$)가 107이고, T 가 10인 경우 평가 값에 따라 다음과 같은 확률을 갖는다.

| Eval($V_n$) | Difference | P    |
| ----------- | ---------- | ---- |
| 80          | 27         | 0.06 |
| 107         | 0          | 0.5  |
| 150         | -43        | 0.99 |


## Temparature changes
SA는 알고리즘이 동작함에 따라 온도를 점차 낮춘다. 위 수도 코드에서 함수 g(T, t)는 다음 온도를 결정하는 함수이다. 구현하기에 따라 다양한 함수가 사용될 수 있다.

1. T = T - 1; (균일하게 온도를 낮추는 경우)
2. T = T - t; (시간에 비례하여 온도를 낮추는 경우)
3. T = T / 2; (지수적으로 온도를 낮추는 경우)

이에 따라 알고리즘 초기에는 나쁜 해를 채택하여 더 다양한 공간을 탐색하게 하며, 이후 점차 낮아지는 온도를 기반으로 최적해를 찾아나간다. 