.을 찍어서 객체 그래프를 탐색하는 것
```
select m.username -> 상태 필드
	from Member m
	join m.team t -> 단일 값 연관 필드
	join m.orders o -> 컬렉션 값 연관 필드
	where t.name = '팀A'
```

# 용어 정리
* 상태 필드*state field* : 단순히 값을 저장하기 위한 필드
* 연관 필드*association field* : 연관관계를 위한 필드
	* 단일 값 연관 필드 : @ManyToOne, @OneToOne, 대상이 엔티티
	* 컬렉션 값 연관 필드 : @OneToMany, @ManyToMany, 대상이 컬렉션

# 경로 표현식 특징

* 상태 필드 : 경로 탐색의 끝, 더 이상의 탐색이 불가능함
* 단일 값 연관 경로: 묵시적 내부 조인*inner join* 발생, 탐색 o
	* 묵시적 join은 내부 조인만 가능하다.
```java
String query = "select m.team from Member m";
String query2 = "select o.member.team from Order o";
//가능, but join이 member를 얻기 위해, team을 얻기 위해 2번 수행된다.

```
![](https://i.imgur.com/8ewFAyQ.png)

* 컬렉션 값 연관 경로 : 묵시적 내부 조인 발생, **탐색 X**
	* FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색할 수 있다.
```java
String query2 = "select t.members.username from Team t"; 
//실패, 컬렉션 값 연관경로에서는 탐색을 진행할 수 없다.
String query = "select t.members from Team t";
List<Collection> result = em.createQuery(query, Collection.class).getResultList();

for(Object o : result) {
	System.out.println(o);
}

String query =  "select m.username from Team t join t.members m";//가능
```

실무에서 묵시적 내부 조인은 자제하는 것이 좋다. 쿼리 튜닝 단계에서 직관적으로 보이지 않아 어려움이 생길 수 있다.
