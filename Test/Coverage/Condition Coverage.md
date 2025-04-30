---
상위 개념: "[[Code Coverage]]"
---
# Condition Coverage
하나의 조건문은 여러 개의 조건으로 이루어질 수 있습니다. Branch Coverage는 조건문의 결과가 true 또는 false가 되었는지만 확인하는 반면, Condition Coverage는 (a < 5 && b > 3)과 같은 복합 조건식에서 각 조건이 true와 false를 모두 경험했는지를 검사합니다.

예를 들어, 위와 같은 복합 조건식에서는 a < 5와 b > 3 각각이 true와 false의 값을 가질 수 있으므로, 가능한 경우의 수는 4가지입니다: true-true, true-false, false-true, false-false. 이 네 가지 경우가 모두 테스트되었을 때, Condition Coverage는 100%가 됩니다.