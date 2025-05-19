---
상위 개념: "[[Database]]"
---
# Key의 종류

## Super Key
> a subset of attributes of relation schema that satisfies Uniqueness

> Every relation has at least one default superkey

**유일성**의 특성을 만족하는 속성 또는 속성들의 집합

### Key
> Attribute ( or attributes combination) that can uniquely identify every single tuples in a relation schema 

후보키(Candidate Key)라고도 불리며, 튜플을 찾거나 정렬할 때 다른 튜플과 구별할 수 있는 기준이 될 수 있는 속성들을 의미한다. 이는 유일성의 특성에 더하여 최소성을 만족시켜야 한다.

### Primary Key
> Chosen Key from candidate keys

후보키 중 기본적으로 사용할 키로 선택한 키를 의미한다. NULL 값을 가질 수 있거나, 값이 자주 변경되거나, 복잡한 값으로 구성된 후보키는 PK로 선택되기에 부적합하다.

### Alternate Key
> The remaining candidate keys

PK로 선택되지 않은 후보키를 의미한다.

### Foreign Key
> An attribute that is referencing the PK of other relation schema

다른 릴레이션의 PK를 참조하는 키


