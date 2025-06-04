---
상위 개념: "[[Normalization]]"
---
# Lossless Decomposition

Decomposition은 분해입니다. Lossless Decomposition은 손실 없는 분해입니다. 데이터베이스를 설계하다보면 여러 이유로 릴레이션을 분리해야할 때가 있습니다. 이는 각 릴레이션의 관심사를 좀 더 세분화하여 분리해야하기 위함일 수도 있고, 중복된 데이터를 없애기 위해 분리할 수도 있습니다. 여러 이유로 릴레이션을 분리할 필요가 있는데, 아무런 규칙 없이 릴레이션을 분리하면 데이터 손실 혹은 중복이 발생합니다.
![](https://i.imgur.com/uegc5Lj.png)

어떤 기준으로 릴레이션을 분리해야 원본의 릴레이션을 그대로 복구할 수 있을까요? 릴레이션 R이 R1과 R2로 분리된다고 했을 때, 이 분리가 손실이나 중복이 발생하지 않기 위해서는 R1과 R2의 교집합 속성이 R1이나 R2에 [함수적 종속성](Functional%20Dependency.md)를 가져야 합니다.


### Example

$R = \{ A, B, C\}$

$FD= \{ A \rightarrow B, B \rightarrow C \}$

$R_1 = \{ A,B \} , R_2 = \{B,C\}$

$R_1 \cap R_2 = \{B\}$

함수적 종속성의 성질 ( if {X}→{Y}, then {X} → {X,Y} ) 로 인해 B → {B,C} 가 성립하므로

위 R1, R2로 분해하는 것은 Lossless Decomposition입니다.
