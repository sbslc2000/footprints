---
상위 링크: "[[Entity Mapping]]"
---
# Inheritance Relation Mapping

관계형 데이터베이스에는 객체에서의 상속 관계는 존재하지 않고, 슈퍼타입 서브타입 관계라는 모델링 기법이 상속과 유사하다.
![](https://i.imgur.com/3n50fzZ.png)


상속관계 매핑이란 객체의 상속 구조와 DB의 슈퍼타입 서브타입 관계를 매핑하는 것이다.

# 매핑 전략
슈퍼타입 서브타입 논리모델을 실제 물리모델로 구현하는 방법에는 3가지가 있다. 서로다른 구현법을 지원하지만 객체 레벨에서는 동일하게 사용할 수 있다.
## 조인 전략
Item이라는 테이블과, 각각의 서브타입 테이블을 만들고 데이터를 가져올 때 JOIN을 사용하는 전략이다.
![](https://i.imgur.com/7zsOJ0w.png)

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public class Item {
	@Id @GeneratedValue
	private Long id;
	private String name;
	private int price;
}

@Entity
public class Album extends Item {
	private String artist;
}

@Entity
public class Movie extends Item {
	private String director;
	private String actor;
}
```

```java
em.persist(movie); //1 
Movie findMovie = em.find(Movie.class, movie.getId()); //2
```
1에서는 객체의 데이터를 서로 다른 테이블에 분산하여 저장한다. 
2에서는 서로 다른 테이블을 조인하여 객체에 매핑하여 반환한다.

### 특징
* DB가 정규화가 잘 되어있다.
* 외래키 참조 무결성 제약조건을 활용할 수 있다.
* 조회 쿼리가 복잡하고 JOIN을 사용하게 되어 성능이 저하될 수 있다.
* 데이터 저장 시 INSERT SQL을 2번 호출해야한다.

### @DiscriminatorColumn
엔티티 레벨에 작성하면 상속관계에 해당하는 컬럼을 만들고 하위 타입의 엔티티 이름을 넣어준다.
```java
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn
public class Item { ... }

@Entity
@DiscriminatorValue("Al") //DiscriminatorColumn에 들어갈 이름을 지정할 수 있다.
public class Album extends Item { ... }
```

조인 전략에서 DB의 ITEM 테이블만 보면 각 레코드가 어떤 하위 타입인지 파악할 수 없기에, 이러한 방법을 사용하여 편의성을 높일 수 있다.

## 단일 테이블 전략
테이블 하나에 모든 컬럼을 놓는 방식으로, JPA가 기본적으로 채택하는 전략이다.
![](https://i.imgur.com/azSxoQC.png)

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
public class Item {
	@Id @GeneratedValue
	private Long id;
	private String name;
	private int price;
}

@Entity
public class Album extends Item {
	private String artist;
}

@Entity
public class Movie extends Item {
	private String director;
	private String actor;
}
```

단일테이블전략에서는 DiscriminatorColumn을 작성하지 않아도 알아서 적용된다.

### 특징 
* JOIN이 필요 없고 쿼리가 단순하여 일반적으로 조회 성능이 좋다.
* 중복 컬럼을 제외하고는 모두 NULL을 허용해주어야 한다.
* 테이블이 비대해져서 발생하는 성능 저하가 생길 수도 있다.
## 구현 클래스마다 테이블 전략
각각의 타입들이 가져야할 값들을 모두 갖고있는 테이블을 각각 만드는 방식이다.

![](https://i.imgur.com/NT1eM7B.png)

```java
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_ CLASS)
public abstract class Item { 
	@Id @GeneratedValue
	private Long id;
	private String name;
	private int price;
}

@Entity
public class Album extends Item {
	private String artist;
}

@Entity
public class Movie extends Item {
	private String director;
	private String actor;
}
```
테이블이 다르기 때문에 Discriminator는 의미가 없다.
```java
Item findItem = em.find(Item.class, movie.getId());
```
위와 같이 상위 클래스로 데이터를 찾는 경우에는 비효율적으로 동작한다.

### 특징
* DBA 측에서도, AP 개발자 입장에서도 추천되지 않는 전략이다.
* NULL 조건을 자유롭게 사용할 수 있다.
* 상위 클래스로 데이터 찾는 경우 UNION을 사용한다.
* 변경이라는 관점에서 좋지 않다. (타입 추가가 될 때 변경의 지점이 많아진다.)


# @MappedSuperclass

DB의 슈퍼타입 - 서브타입 관계로 나누고 싶지는 않지만 , 객체 레벨에서 공통 매핑 정보가 필요할 때 사용한다.

```java
@MappedSuperclass //매핑정보만 제공하는 슈퍼클래스
public abstract class BaseEntity {
	private LocalDateTime createdAt;
	private LocalDateTime updatedAt; 
}
```

@MappedSuperclass가 사용된 클래스는 자식에게 매핑 정보만 제공하며, 조회, 검색의 대상이 되지 않는다. 직접 생성해서 사용할 일이 없으므로 추상클래스로 만들 것을 권장한다.

참고: @Entity 클래스는 엔티티나 @MappedSuperclass로 지정한 클래스만 상속이 가능하다.



