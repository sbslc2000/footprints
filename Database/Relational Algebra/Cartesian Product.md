---
상위 개념: "[[Relational Algebra]]"
---
# Cartesian Product
두 릴레이션 R과 S의 카르테시안 프로덕트는 다음과 같이 표현한다.
$$ R \bigtimes S$$

$R \bigtimes S$는 릴레이션 R에 속한 각 튜플과 릴레이션 S에 속한 각 투플을 모두 연결하여 만들어진 새로운 튜플로 결과 릴레이션을 구성한다.


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
$R \bigtimes S$

| R.ID | R.name | S.ID | S.name |
| ---- | ------ | ---- | ------ |
| 100  | Pete   | 100  | Pete   |
| 100  | Pete   | 400  | Lee    |
| 200  | John   | 100  | Pete   |
| 200  | John   | 400  | Lee    |
| 300  | James  | 100  | Pete   |
| 300  | James  | 400  | Lee    |

 