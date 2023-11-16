* 문자 : 'HELLO', 'She''s'
* 숫자 : 10L , 10D, 10F
* Boolean : TRUE, FALSE
* ENUM: jpabook.MemberType.Admin (패키지 명 포함)
* 엔티티 타입 : TYPE(m) = Member (상속 관계에서 사용)

## 예시

ENUM 타입
```java
//멤버의 타입이 Admin인 모든 Member 엔티티를 조회
String query = "select m from Member" +
	"where m.type = org.example.app.UserType.Admin";

//타입을 파라미터로 바인딩할 때
String query = "select m from Member" +
	"where m.type = :userType";

em.createQuery(query)
	.setParameter("userType",UserType.ADMIN).
	getResultList();

//엔티티타입 -> DType 분류 시
String query2 = "select i from Item i " +
	"where type(i) == Book";

em.createQuery(query2, Item.class);
```