---
상위 개념: "[[Relational Algebra]]"
---
# Division
릴레이션 R, S에 대한 디비전 연산은 S의 모든 투플과 관련 있는 릴레이션 R의 튜플로 결과 릴레이션을 구성한다. 다음과 같이 표기한다.
$$R \div S$$

릴레이션 R이 릴레이션 S의 모든 속성을 포함하고 있어야 연산이 유효하다.

## Example
Relation Enroll

| STUDENT | SUBJECT |
|---------|---------|
| Alice   | DB      |
| Alice   | OS      |
| Alice   | AI      |
| Bob     | DB      |
| Bob     | OS      |
| Charlie | DB      |
Relation SubjectList

| SUBJECT |
| ------- |
| DB      |
| OS      |

$$ Enroll \div SubjectList $$

| STUDENT |
|---------|
| Alice   |
| Bob     |