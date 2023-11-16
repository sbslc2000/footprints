# 서브 쿼리 지원 함수
* \[NOT] EXISTS (subquery): 서브쿼리에 결과가 존재하면 참
* {ALL | ANY | SOME} (subquery)
	* ALL 모두 만족하면 참
	* ANY, SOME : 조건을 하나라도 만족하면 참
* \[NOT] IN (subquery): 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참
## 예제
* 팀A 소속인 회원  
`select m from Member m where exists (select t from m.team t where t.name = ‘팀A')`

* 전체 상품 각각의 재고보다 주문량이 많은 주문들
`select o from Order o where o.orderAmount > ALL (select p.stockAmount from Product p)`

* 어떤 팀이든 팀에 소속된 회원
`select m from Member m where m.team = ANY (select t from Team t)`

## 한계

JPA 표준 명세에서는 WHERE, HAVING 절에서만 서브쿼리 사용 가능하다.
하지만 Hibernate에서는 SELECT 절에도 서브쿼리를 사용 가능하다.

반면, **FROM 절의 서브쿼리는 현재 JPQL에서는 불가능**하여 조인으로 해결해야한다.
(혹은 쿼리를 두번 날리거나 native 쓰자)

(근데 Hibernate 6에서는 FROM절 서브쿼리 지원함! 22년 06월 업뎃)