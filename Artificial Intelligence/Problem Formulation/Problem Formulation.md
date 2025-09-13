# Problem Formulation

## Problem
인공지능에서의 문제는 4개의 속성으로 구성된다.

1. Initial State, Initial Input
2. Sucessor Function: set of action-state pairs 
3. goal test
4. path cost: performance measure

## Solution
> A 'solution' is a sequence of actions leading from the initial state to a goal state

AI 관점에서의 솔루션이란 초기 상태에서부터 목표 상태까지 이동시키는 행동들의 연속들을 의미한다.

## Problem Formulation
### Example Romania
1. Initial State: x = 'at Arad'
2. Sucessor function: S = Arad -> ... -> ... -> Bucharest
3. goal test: x = 'at Bucharest'
4. path cost: sum of distances of S

### Example Vacuum cleaner 
1. Initial state: integer dirt and robot location 
2. Sucessor function: left, right, suck, stay (sequence)
3. Goal Test: no dirt (left, right rooms)
4. Path Cost (performance measure)
	1. +1 per action
	2. -1 per remaining dirt

### Example Robot Assembly
1. Initial State: real-valued coordinates of joint angles
2. Successor function: motion moves 
3. Goal Test: complete assembly
4. Path Cost: time to execute, reliable

### Example 8-puzzle
1. Initial State: current tile
2. Successor Function : move sequence
3. Goal Test : goal tile set
4. Path Cost: time - frequency

## Problem Solving
결론적으로 인공지능에서의 '문제 해결'이란, 초기 상태에서 시작해서 다양한 상태 공간들을 통과하여 목표 상태를 달성하는 것을 의미한다.

AI의 문제해결은 이론적인 영역에서 봤을 때, 여러 상태 공간들을 트리 탐색하는 것으로 볼 수 있다. 누가 더 트리 탐색을 효과적으로 하느냐가 더 좋은 솔루션을 더 빠르게 찾는지를 결정한다.


