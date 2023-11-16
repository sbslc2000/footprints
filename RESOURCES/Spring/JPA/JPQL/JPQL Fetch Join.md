JPQL의 fetch join이란 JPQL에서 성능 최적화를 위해 제공하는 기능이다. SQL Join의 종류가 아니며, 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능을 제공한다. Fetch Join으로 가져오는 엔티티 값은 FetchType 전략보다 우선하여 작동한다.

최적화시 글로벌 로딩 전략 (FetchType)을 모두 지연로딩으로 적용시키고, 최적화가 필요한 곳을 Fetch Join을 사용하는 것이 좋다.

Fetch Join은 객체 그래프를 유지할 때 사용하면 효과적이며, 만약 조인을 통해 엔티티가 아닌 전혀 다른 결과를 내야하는 경우 일반조인을 사용하여 진행하는 것이 효과적이다.

# Entity Fetch Join
회원을 조회하면서 연관된 팀도 함께 조회하려면 어떻게 해야할까?

\[JPQL]
select m from Member m join fetch m.team

\[SQL]
SELECT M.* , T.* FROM Member M INNER JOIN Team T on M.TEAM_ID=T.ID

## 예제 
Member 엔티티와 Team 엔티티가 존재한다. 회원 1과 회원 2는 Team 1 소속, 회원 3은 팀 2 소속, 그리고 회원 4는 팀에 소속되어있지 않다.

![](https://i.imgur.com/Dc6zAEC.png)

```java
String query = "select m from Member m join fetch m.team";

List<Member> members = 
em.createQuery(query, Member.class).getResultList();

for(Member m : members) {
	System.out.println("m.teamName", m.getTeam().getName());
	//이 시점에서 Team은 이미 영속성 컨텍스트에 저장되어 있음
}
```

fetch join을 사용하면 쿼리를 날리는 시점에 inner join을 통해 연관관계에 있는 Team 엔티티의 정보를 함께 가져온다. Outer Join을 사용하고 싶다면 다음과 같이 작성하면 된다.
`String query = "select m from Member m left join fetch m.team"`

# Collection Fetch Join

Team 입장에서 다측에 해당하는 엔티티의 정보를 한번에 가져오고자 할 때 사용한다.
```java
String query = "select t from Team t join fetch t.members";

List<Team> teams = 
	em.createQuery(query,Team.class).getResultList();

for(Team t : teams) 
	t.getMembers().forEach((m) -> System.out.println(m.getName());
```

## 주의사항
Collection Fetch Join을 하면 다 측 데이터가 늘어나는 상황이 발생할 수 있다. 이는 JOIN의 특성에 의해서 발생한다.
![](https://i.imgur.com/TjaHzeD.png)
JOIN을 하면 팀A와 관련된 레코드가 멤버의 개수만큼 나타나듯이, Collection Fetch Join의 결과로도 컬렉션에 Team A 엔티티가 일 측의 개수만큼 저장된다. 
![](https://i.imgur.com/JwxyGCD.png)
물론 영속성 컨텍스트 내에는 올바르게 저장된다.

## DISTINCT
위와 같은 문제를 해결하기 위해서는 JPQL DISTINCT를 사용할 수 있다.
```sql
select distinct t from Team t join fetch t.members;
```
DISTINCT를 사용하면 SQL 레벨로 번역될 때 역시 distinct가 들어가지만, 동일한 레코드는 존재하지 않을 것이므로 중복제거 되는 레코드는 없다.
![](https://i.imgur.com/7xkNLRG.png)

이후 DISTINCT는 애플리케이션 레벨에서 JPA가 확인하여 중복된 엔티티를 없애준다.
>[!info]
>Hibernate 6 버전 부터는 DISTINCT를 사용하지 않아도 애플리케이션에서 중복 제거가 자동으로 적용된다!

# Fetch Join과 일반 조인의 차이
**일반 조인 실행시 연관된 엔티티를 함께 조회하지 않음**
```java
String query = "select t from Team t join t.members m";

List<Team> result =
	em.createQuery(query,Team.class).getResultList();
```
일반 조인의 경우 join은 내부 조인으로만 작동하여 t.members 가 존재하는 Team들만 필터링된 결과가 나온다. 쿼리의 반환값은 Team 엔티티의 데이터(select 절에 지정한 것) 뿐이고, 따라서 이후 members를 조회할 때에는 쿼리가 나간다.
![](https://i.imgur.com/SNSIPxk.png)

반면 Fetch Join을 사용한다면 연관된 엔티티의 정보도 쿼리를 통해 함께 조회한다. (일종의 즉시로딩)

# 한계
1. **Fetch Join의 대상에는 별칭을 줄 수 없다. Hibernate는 가능하나, 가급적 사용하지 않는게 좋음**

`select t from Team t join fetch t.members m where m.age < 34`
이걸 필터링해서 가져온다고 할 때, 하나의 엔티티가 영속성 컨텍스트에서 관리될 때 충돌이 발생할 것이다. 

위와 같은 작업을 하고 싶다면, Team에서 Member를 가져오는 것이 아닌 Member 로부터 데이터를 가져오는 쿼리를 따로 날리는 방법으로 풀어내야 한다. 객체 그래프는 모든 데이터를 가져오는 것으로 설계되어있으니 그 원칙을 어기는 것이 좋아보이지 않는다.

2. **둘 이상의 컬렉션은 Fetch Join 할 수 없다.**

```
select t from Team t join fetch t.members join fetch t.XXX
```
데이터 뻥튀기 문제로 정합성이 깨질 가능성이 높다.

3. **컬렉션을 Fetch Join 하면 페이징 API(setFirstResult, setMaxResults) 를 사용할 수 없다.**

이 역시 데이터 뻥튀기 현상에 의한 것임.
![](https://i.imgur.com/7xkNLRG.png)

물론 엔티티 fetch join의 경우 페이징을 사용 가능하다. (데이터 뻥튀기가 안일어나기 때문에). Hibernate의 경우 경고 로그를 남기고 메모리로 다 끌고 온 뒤 애플리케이션 레벨에서 페이징을 수행하긴 하지만, 무결성에 있어서 위험하므로 사용하지 않는 것이 좋다.

[[XXToMany 관계에서 페이징 사용하기]]

이를 해결하려면 방향을 뒤집어서 엔티티 fetch join으로 만들고 페이징을 수행하거나, join문 자체를 명시하지 않을 수 있다. 이 경우 fetch join이 수행되지 않으므로 lazy loading에 의하여 쿼리문이 페이징 개수만큼 추가될 것이다. 이 때, @BatchSize 애노테이션을 컬렉션 매핑 측에 설정을 해준다면, lazy loading을 할 때 bulk 연산을 수행하게 되므로 쿼리의 수를 줄일 수 있다.