---
상위 개념: "[[Normalization]]"
---
# Functional Dependency
함수적 종속성은 데이터베이스에서 속성들 간의 관계를 나타내는 개념입니다. 한 속성 또는 속성 집합의 값에 의해 다른 속성 또는 속성 집합의 값이 유일하게 결정되는 경우, 이를 함수적 종속성이 있다고 합니다. 이를 표현할 때는 X → Y로 표기하며, X를 결정자(Determinant)라고 하고 Y를 종속자(Dependent)라고 합니다.

![](https://i.imgur.com/I7XcoBy.png)


위 릴레이션에서 name은 dept_name 에 대하여 함수적 종속성을 갖습니다. (name → dept_name) name의 값에 따라서 dept_name이 유일하게 결정되기 때문입니다. 반면 dept_name → name 은 함수적 종속성을 갖지 않습니다. dept_name의 값 중 Physics에 대해서는 name의 값이 유일하게 결정되지 않기 때문입니다(James와 Tom이 올 수 있음).

위 릴레이션에서 name→ salary 는 함수적 종속성을 갖습니다. salary → name 역시 함수적 종속성을 갖습니다. 하지만 이 관계는 앞으로도 쭉 함수적 종속성을 가질까요? 그렇지 않을 것입니다. 5번 투플에 {5, Seo, Comp.Sci. , 95000} 의 투플이 들어온다면 salary 는 name에 대해서 함수적 종속성을 갖지 못합니다. FD 를 판단할 때에 유의할 점은, 현재 시점에 릴레이션에 포함된 속성 값만으로 판단하면 안된다는 점입니다. 릴레이션에서 속성 값은 계속 변할 수 있기 때문에 속성 자체가 갖고 있는 특성과 의미를 기반으로 판단해야합니다.

id 는 {name, dept_name, salary}에 대해서 함수적 종속성을 갖습니다. 이렇게 하나의 속성 혹은 속성 집합이 나머지 속성 집합에 함수적 종속성을 갖는다면 전자의 속성들은 후보키가 될 수 있습니다.

## Fully Functional Dependency

Fully Functional Dependency, FFD 는 릴레이션에서 속성 집합 Y가 속성 집합 X에 함수적으로 종속되어있지만, 속성 집합 X의 전체가 아닌 일부분에는 종속되지 않음을 의미합니다.

## Partial Functional Dependency

PFD는 속성 집합 Y가 속성 집합 X의 전체가 아닌 일부분에도 함수적으로 종속됨을 의미하므로, 부분 함수 종속 관계가 성립하려면 결정자가 여러개의 속성들로 구성되어 있어야 합니다.

FFD 와 PFD의 개념을 읽기만 한다면 무슨 이야기를 하는지 와닿지 않을 겁니다. 아래 예시를 보고 위 문장들을 다시 곱씹어 본다면 어렵지 않게 이해할 수 있을 겁니다.

## FFD and PFD 예시

GRADE = F(ID,COURSE_ID)
![](https://i.imgur.com/OwzaaG9.png)


위에 있는 테이블은 학생 ID와 학년, 수강과목과 점수를 담고 있는 릴레이션이고, 아래 다이어그램은 속성 간의 연결관계를 표현한 다이어그램입니다.

ID와 COURSE_ID 속성의 집합은 SCORE라는 속성에 대해 Functional Dependency를 갖습니다. 또한 이 관계는 Fully Functional Dependency 관계인데, 그 이유는 속성 {ID, COURSE_ID} 가 속성 {SCORE}에 대해 FD를 갖고 있으면서, {ID,COURSE_ID} 집합의 부분집합인 {ID} 나 {COURSE_ID} 는 {SCORE}과 FD를 갖지 않기 때문입니다.

{ID,COURSE_ID}는 {GRADE}에 대해 Functional Dependency를 갖습니다. 이 관계는 Partial Functional Dependency인데, 그 이유는 {ID,COURSE_ID}의 부분집합인 {ID} 가 {GRADE}와 FD를 갖기 때문입니다.