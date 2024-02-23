---
상위 링크: "[[JPQL]]"
---
# JPQL 조건식

## 기본 CASE 식
```java
String query = "select " +
		"case when m.age <=10 then '학생요금'" +
		"     when m.age >= 60 then '경로요금'" +
		"     else '일반요금' end " +
	"from Member m";

List<String> result = em.createQuery(query,String.class);
```
## 단순 CASE 식
```java
select
case t.name
when '팀A' then '인센티브110%' when '팀B' then '인센티브120%'
end
else '인센티브105%'
from Team t

List<String> result = em.createQuery(query,String.class);
```

## COALESCE

하나씩 조회해서 null이 아니면 반환. NULL
`select coalesce(m.username, "이름 없는 회원");`

m.username이 null이라면 다음 괄호 값을 조회한다. 다음 괄호 값은 "이름 없는 회원"이므로 null이 아니어서 반환됨

## NULLIF

두 값이 같으면 null 반환, 다르면 첫번째 값(아래에서는 m.username) 반환 
`select NULLIF(m.username, '관리자') from Member m;`