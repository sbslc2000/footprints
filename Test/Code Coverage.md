---
상위 링크: "[[Testing]]"
---
# Code Coverage

Code Coverage란 소프트웨어 테스트를 수행했을 때 소스 코드가 얼마나 정밀하게 테스트에 의해 실행되었는가와 관련된 척도입니다. 이 척도에는 여러가지 기준이 있어서 다양한 관점으로 테스트 결과를 평가할 수 있습니다.

## Function Coverage
Function Coverage는 소스코드 내에 있는 function들의 개수를 토대로 테스트를 평가합니다. 100줄의 소스코드 내에서 함수가 5개가 존재했고, 그 중 3개만 테스트 도중 호출되었다면 Function Coverage는 60%입니다.

## Line Coverage
Line Coverage는 테스트에 의해서 소스코드의 한 줄 한 줄이 실행되었는가 아니었는가를 표현합니다. 전체 소스코드가 100줄이고, 테스트를 통해 그 중 80개의 라인이 실행되었으면 Line Coverage는 80%입니다.

## Branch Coverage
Branch Coverage는 if 문과 같은 분기문에서 각각의 분기 경로가 테스트를 통해 실행되었는지를 평가하는 기준입니다. 예를 들어, 코드 내에 if - else 문이 10개 있다면, 각 조건에 따라 가능한 분기 경로는 총 20개가 됩니다. Branch Coverage는 “실행된 분기 경로의 수 / 전체 분기 경로의 수”로 커버리지를 계산합니다.

## Condition Coverage
하나의 조건문은 여러 개의 조건으로 이루어질 수 있습니다. Branch Coverage는 조건문의 결과가 true 또는 false가 되었는지만 확인하는 반면, Condition Coverage는 (a < 5 && b > 3)과 같은 복합 조건식에서 각 조건이 true와 false를 모두 경험했는지를 검사합니다. 예를 들어, 위와 같은 복합 조건식에서는 a < 5와 b > 3 각각이 true와 false의 값을 가질 수 있으므로, 가능한 경우의 수는 4가지입니다: true-true, true-false, false-true, false-false. 이 네 가지 경우가 모두 테스트되었을 때, Condition Coverage는 100%가 됩니다.