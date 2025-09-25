---
상위 링크: "[[JPQL]]"
---
# JPQL Join
## 문법

* **내부 조인**
SELECT m FROM Member m \[INNER] JOIN m.team t

* **외부 조인**
SELECT m FROM Member m LEFT \[OUTER] JOIN m.team t

* **세타 조인**
select count(m) from Member m, Team t where m.username = t.name
이 경우 cartesian product 결과에 where절이 필터링 된 결과 릴레이션 반환

## 예시
```java
String query = "select m from Member m inner join m.team t";
List<Member> result = em.createQuery(query, Member.class);
```

이 경우, Team이 존재하는 Member만 가져올 수 있다. 만약 이 상황에서 다대일 매핑에 Lazy Loading을 설정해주지 않는다면, Member를 사용하지 않아도 Team을 가져오는 쿼리가 나간다.

```java
String query = "select m from Member m left join m.team t"; //left join

String query = "select m from Member m, Team t where m.username = t.name"; //Theta Join
List<Member> result = em.createQuery(query, Member.class).getResultList();
```

## ON 절
JPA 2.1부터는 ON 을 사용할 수 있다.
ON은 조인 대상을 필터링할 뿐만 아니라, 연관관계가 없는 엔티티도 외부 조인을 가능하게 한다.

### 조인 대상 필터링
예) 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인

JPQL:
`SELECT m, t FROM Member m LEFT JOIN m.team t on t.name = 'A'`
=> Team 이름이 A인 레코드만을 left join할 것이다.
=> sql : `SELECT m.* ,t.* FROM Member m LEFT JOIN Team t ON m.TEAM_ID=t.id and t.name='A'`

### 연관관계 없는 엔티티 외부 조인
예) 회원의 이름과 팀의 이름이 같은 대상 외부 조인

JPQL:
`SELECT m, t FROM  
Member m LEFT JOIN Team t on m.username = t.name`

SQL:
`SELECT m.*, t.* FROM  
Member m LEFT JOIN Team t ON m.username = t.name`