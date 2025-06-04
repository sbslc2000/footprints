---
상위 개념: "[[Relational Algebra]]"
---
# UNION
$$ R \cup S$$
릴레이션 R과 S의 합집합은 UNION 연산자를 통해 표현할 수 있다.

합집합을 한 후 얻어지는 결과 릴레이션의 차수는 피연산자인 릴레이션 R과 S의 차수와 같다. 카디널리티는 중복이 없기 때문에 릴레이션 R과 S의 투플 개수를 더한 것과 같거나 적어진다.

$R \cup S$와 $S \cup R$의 결과는 같다.

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
$R \cup S$

| ID  | name  |
| --- | ----- |
| 100 | Pete  |
| 200 | John  |
| 300 | James |
| 400 | Lee   |
 