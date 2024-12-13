---
상위 개념: "[[Database]]"
---
# Key의 종류

### Super Key
> a subset of attributes of relation schema that satisfies Uniqueness

> Every relation has at least one default superkey

**유일성**의 특성을 만족하는 속성 또는 속성들의 집합

### Key

> Attribute ( or attributes combination) that can uniquely identify every single tuples in a relation schema 

튜플을 찾거나 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 기준이 되는 attribute

> Key is minimal superkey for given attributes combination Minimum

**유일성**과 **최소성**을 만족하는 속성 또는 속성들의 집합이며, Candidate Key라고도 불린다.

### Primary Key
> Chosen Key from candidate keys

Key 중 기본적으로 사용할 키로 선택한 키를 의미한다.

### Alternate Key
> The remaining candidate keys

PK로 선택되지 않은 Candidate Key를 의미한다.

### Foreign Key
> An attribute that is referencing the PK of other relation schema

다른 릴레이션의 기본키를 그대로 참조하는 속성의 집합


