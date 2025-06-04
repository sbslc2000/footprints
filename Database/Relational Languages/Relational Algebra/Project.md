---
상위 개념: "[[Relational Algebra]]"
---
# Project
프로젝트 연산은 릴레이션에서 선택한 속성에 해당하는 값으로 결과 릴레이션을 구성하며, 다음과 같이 표현한다. 결과 릴레이션은 주어진 릴레이션의 일부 열로만 구성되어 해당 릴레이션의 수직적 부분 집합(vertical subset)을 생성하는 것과 같다.
$$ \Pi_{attribute\_list}(Relation)$$



## Example
Sample Relation Instructor 

| ID    | name   | age | department    | salary |
| ----- | ------ | --- | ------------- | ------ |
| 10000 | Pete   | 25  | Comp. Science | 20000  |
| 20000 | Paek   | 30  | Comp. Science | 40000  |
| 30000 | Homer  | 40  | Management    | 30000  |
| 40000 | Maggie | 50  | Biology       | 40000  |
| 50000 | Bart   | 60  | Biology       | 50000  |

$$ \Pi_{ID, name, age}(Instructor) $$

| ID    | name   | age |
| ----- | ------ | --- |
| 10000 | Pete   | 25  |
| 20000 | Paek   | 30  |
| 30000 | Homer  | 40  |
| 40000 | Maggie | 50  |
| 50000 | Bart   | 60  |
