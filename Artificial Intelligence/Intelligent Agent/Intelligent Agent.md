---
상위 개념: "[[Artificial Intelligence]]"
---
# Intelligent Agent
> “센서를 통해 환경을 받아들이고, 액추에이터를 통해 환경에 작용하는 것”

> A rational agent chooses whichever action maximizes the "expected" value of the "performance measure" given the percept sequence.

Intelligent Agent(지능형 에이전트)란 자율적으로 환경을 인식하고, 그에 맞게 행동을 취하여 목표를 달성하는 시스템을 의미한다. 즉, 에이전트는 입력(Percepts)을 받아들이고, 이를 기반으로 합리적인 행동(Actions)을 선택하는 구조를 가진다.

$$ f: P \rightarrow A $$

> [!info] 영화 Matrix : Neo vs. Agent Smith
> 여기서 Agent는 단순한 요원이라는 의미를 넘어 인공지능 프로그램이라는 의미를 담고 있다.


## Ex: Vacuum Cleaner Agent
* perception (input): 현재 어떤 방에 있는지, 방 안에 먼지가 얼마나 쌓여있는지, ... 
* action (output): 이동, 흡입, ...
* performance measures
	* -1 point per move
	* +1 point when fast finish 
	* -1 point 남은 먼지량
	* 배터리 소모량 
	* ...

## 오해 : "rational agent != perfect"
- 합리성(Rationality)은 **제한된 정보와 불확실한 환경 속에서 최선의 의사결정을 하는 것**이다.
- 전지전능(omniscient)처럼 모든 걸 알지도 않고, 예지력(clairvoyant)처럼 미래를 확정적으로 예측하지도 못한다.
- 대신, 그 순간의 정보와 경험, 환경 모델을 기반으로 기대되는 성과를 최대화하는 방향으로 행동합니다.