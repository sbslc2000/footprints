JPQL은 Java Persistence Query Language의 약자로 SQL과 비슷한 문법을 제공하며, 쿼리를 작성할 때 테이블이 아닌 엔티티 객체를 대상으로 할 수 있게 만든 **객체 지향 쿼리 언어**이다. SQL을 추상화하여 [특정 데이터베이스 SQL](Database%20Dialect)에 의존하지 않는다.
 
```java
List<Member> members = em.createQuery(
	"select m from Member m where m.username like '%kim%'", 
	Member.class)
	.getResultList();
```
JPQL은 SQL로 번역되어 수행된다. 

# 문법
## 기본

```
select:

select
	from
	[where]
	[groupby]
	[having]
	[orderby]

update:

update
	[where]


```

* `select m from Member as m where m.age > 18`
* 엔티티와 속성은 대소문자를 구분해야한다. (객체와 동일하게)
* JPQL 키워드는 대소문자를 구분하지 않는다.
* 테이블의 이름이 아닌 엔티티의 이름을 사용해야한다.
	* 기본적으로 클래스 이름, 혹은 Entity 애노테이션의 속성 name
* 별칭은 필수이며, as는 생략 가능

## 집합과 정렬
```
select
	COUNT(m),
	SUM(m.age),
	AVG(m.age),
	MAX(m.age),
	MIN(m.age)
	from Member m;
```

## TypeQuery, Query
* **TypeQuery** : 반환 타입이 명확할 때 사용
* **Query** : 반환 타입이 명확하지 않을 때 사용
```java
TypedQuery<Member> query = em.createQuery("select m from Member m", Member.class);

TypedQuery<String> query2 = em.createQuery("select m.username from Member m", String.class)

Query query3 = em.createQuery("select m.username, m.age from Member m");
```

## 결과 조회 API
```java
TypedQuery<Member> query = em.createQuery("select m from Member m", Member.class);
List<Member> members = query.getResultList();

TypedQuery<Member> query2 = em.createQuery("select m from Member m where m.id = 1");
Member member = query2.getSingleResult();
```

getResultList는 결과가 없다면 빈 리스트를 반환한다. 반면 getSingleResult에서는 결과가 없다면 NoResultException, 둘 이상이라면 NonUniqueResultException을 반환한다.

## 파라미터 바인딩
파라미터를 이름 기준, 위치 기준으로 바인딩 할 수 있다.
```java
//select * from Member m where m.username = 'member1' 

//이름 기준
TypedQuery<Member> query = em.createQuery(
	"select m from Member m where m.username = :username", Member.class
);

query.setParameter("username","member1");
List<Member> members = query.getResultList();

//위치 기준
TypedQuery<Member> query = em.createQuery(
	"select m from Member m where m.username = ?1", Member.class
);

query.setParameter(1, "member1");
List<Member> members2 = query.getResultList();
```
위치 기준보다 이름을 기준으로 할 것을 권장한다. 위치기준은 밀리는 문제가 쉽게 발생함.

### 하위 문서
[[JPQL Projection]]
[[JPQL Paging]]
[[JPQL Join]]
[[JPQL Subquery]]
[[JPQL 타입 표현]]
[[JPQL 조건식]]
[[JPQL 경로 표현식]]
[[JPQL Fetch Join]]
