---
상위 링크: "[[Link Multiplexing]]"
---
# Time Division Multiplexing

Time Division Multiplexing, 시분할 다중화, TDM은 동일한 크기의 시간을 나누고 Round-Robin을 사용하여 각각의 노드에게 데이터를 보낼 수 있는 기회를 주는 방법이다.

FDM이 주파수를 통해 링크를 공유했다면 TDM은 시간을 통해 링크를 공유한다. FDM은 인접한 주파수에 간섭과 같은 문제를 신경써야하지만 TDM은 그런거 없다.

하지만 각 시간 슬롯에 대해 사용할 주체가 고정적으로 할당되어야 하는데, 만약 해당 타임 슬롯에 전송할 데이터가 없다면 그 시간 슬롯은 비어있는 상태로 유지된다. 이 때에는 다른 사용자가 링크를 사용하고자해도 보낼 수 없이 유휴상태로 남는다. 

![](https://i.imgur.com/7HRPne8.png)
