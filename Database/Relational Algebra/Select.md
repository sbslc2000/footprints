---
상위 개념: "[[Relational Algebra]]"
---
# Select
셀렉트 연산은 릴레이션에서 주어진 조건을 만족하는 튜플만 선택하여 결과 릴레이션을 구성하며, 다음과 같이 표현한다. 결과 릴레이션은 주어진 릴레이션을 수평으로 절단한 것과 같아 해당 릴레이션의 수평적 부분 집합(horizontal subset)을 생성하는 것과 같다.
$$ \sigma_{condition}(Relation) $$

조건식은 비교 연산자를 통해 구성되며, 조건식은 비교식 혹은 predicate라고도 부른다. 여러개의 비교 연산자와 함께 논리 연산자($\land$, $\lor$, $\lnot$)를 사용하여 더 복잡한 조건식을 구성할 수도 있다.

## Example
Sample Relation Instructor 

| ID    | name   | age | department    | salary |
| ----- | ------ | --- | ------------- | ------ |
| 10000 | Pete   | 25  | Comp. Science | 20000  |
| 20000 | Paek   | 30  | Comp. Science | 40000  |
| 30000 | Homer  | 40  | Management    | 30000  |
| 40000 | Maggie | 50  | Biology       | 40000  |
| 50000 | Bart   | 60  | Biology       | 50000  |

$$ \sigma_{salary > 30000}(Instructor) $$

| ID    | name   | age | department    | salary |
| ----- | ------ | --- | ------------- | ------ |
| 20000 | Paek   | 30  | Comp. Science | 40000  |
| 40000 | Maggie | 50  | Biology       | 40000  |
$$ \sigma_{salary < 30000 \lor department = biology}(Instructor) $$


| ID    | name   | age | department    | salary |
| ----- | ------ | --- | ------------- | ------ |
| 30000 | Homer  | 40  | Management    | 30000  |
| 40000 | Maggie | 50  | Biology       | 40000  |
| 50000 | Bart   | 60  | Biology       | 50000  |
