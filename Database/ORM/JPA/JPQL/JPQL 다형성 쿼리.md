---
상위 링크: "[[JPQL]]"
---
# JPQL 다형성 쿼리
## Type

Type을 사용하면 조회 대상을 특정 자식으로 한정할 수 있다.

\[JPQL]

select i from Item i  
where type(i) IN (Book, Movie)

\[SQL] 
select i from i  
where i.DTYPE in (‘B’, ‘M’)


## Treat
자바의 타입 캐스팅과 유사하며, 상속 구조에서 부모 타입을 특정 자식 타입으로 다룰 때 사용

\[JPQL]

select i from Item i  
where treat(i as Book).author = ‘kim’

\[SQL]

select i.* from Item i  
where i.DTYPE = ‘B’ and i.author = ‘kim’