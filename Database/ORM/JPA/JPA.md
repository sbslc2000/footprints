---
상위 링크: "[[Spring]]"
---
# JPA
JPA는 Java Persistence API 의 약자로, 자바 진영 [[ORM]]의 기술 표준으로 사용하는 인터페이스의 모음입니다. 말 그대로 API이기에, JPA는 JPA 자체로 동작할 수 있는 것이 아닙니다. 자바 개발자들 사이에서 ORM 기술을 이러이러한 API로 사용하면 좋겠다는 약속이며, 이것을 구현한 여러 구현체들이 존재하여 원하는 것을 가져다 쓸 수 있습니다.
![[Pasted image 20230921184158.png]]
JPA는 직접 DB와 소통하는 기능을 갖고 있지 않습니다. JPA는 내부적으로 JDBC를 사용하여 DB와 소통합니다. JPA의 역할은 객체의 영속화 관리이며, 그 과정에서 Entity를 분석하고 SQL문을 만들며 객체와 관계형 모델간 매핑을 담당합니다.
![[Pasted image 20230921184206.png]]
JPA를 구현한 구현체에는 [[Hibernate]], EclipseLink, DataNucleus가 있습니다. 이 중 Hibernate는 spring-boot-starter-jpa 에서 기본적으로 제공하는 라이브러리입니다.

## JPA에서 제공하는 기능
JPA는 Object와 Relational Model의 패러다임 차이를 중간에서 해소하기 위해 사용됩니다. 따라서 JPA는 Relational Model의 개념들을 OOP로 끌고오는 주요 기능들을 갖고 있습니다.
흔히 ERD라고 불리는, Entity-Relation Diagram은 데이터베이스 모델을 추상화하여 표현하는 다이어그램입니다. 데이터 모델은 Entity와 Relation으로 추상화되며, JPA에서도 이 Entity와 Relation을 Object로 변환하는 기능을 제공합니다.
[[Relational Mapping/Entity Mapping]]에서는 JPA가 어떻게 데이터베이스 개체들을 Object와 매핑시키는지에 대해 설명합니다.
[[Relational Mapping/Primary Key Mapping]]에서는 Entity Mapping 중에서도 특히 Primary Key의 매핑을 중심적으로하여 설명합니다.
[[Relational Mapping/Relational Mapping]]에서는 데이터베이스 테이블간 Relation에 대해 어떻게 Object 레벨에서 매핑시키는지에 대해 설명합니다.

## JPA에서 지원하는 쿼리 방법
* [[JPQL/JPQL]]
* [[QueryBuilder/JPA Criteria]]
* [[QueryBuilder/QueryDSL]]
* 네이티브 SQL
	* JPQL로 해결할 수 없는 특정 데이터베이스에 의존적인 기능을 사용할 때
	* `em.createNativeQuery("select MEMBER_ID, city, street, zipcode from MEMBER")`
* JDBC API 직접 사용, MyBatis, SpringJdbcTemplate 함께 사용
	* 이 기능들을 사용하기 위해서는 영속성 컨텍스트를 적절한 시점에 강제로 flush 해야함. (영속성 컨텍스트에만 있고 DB에 반영되지 않은 상태에서 작업이 이루어짐)

# 하위문서
[[영속성 전이 + 고아 객체 => 생명주기]]

[[Persistence Context/JPA 프록시 매커니즘]]
[[Persistence Context/Persistence Context]]
[[Persistence Context/entitymanager/EntityManagerFactory]]
[[Relational Mapping/JPA Value Type]]

### 함께  보기
[[etc/JPA 로깅]]