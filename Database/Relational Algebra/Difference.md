# Difference
합병이 가능한 두 릴레이션 R과 S의 차집합은 다음과 같이 표현한다.
$$ R - S $$

$R - S$는 릴레이션 R에는 존재하지만 릴레이션 S에는 존재하지 않는 투플들로 결과 릴레이션을 구성한다.

차집합은 피연산자의 순서에 따라 결과 릴레이션이 달라지기 때문에 교환적 특징은 물론 결합적 특징도 없다.

## Example

Relation R

| ID  | name  |
| --- | ----- |
| 100 | Pete  |
| 200 | John  |
| 300 | James |
Relation S

| ID  | name |
| --- | ---- |
| 100 | Pete |
| 400 | Lee  |
$R - S$

| ID  | name  |
| --- | ----- |
| 200 | John |
| 300 | James |

 