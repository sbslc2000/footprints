JPA Data Type은 크게 Entity Type과 Value Type으로 나눌 수 있다.

## Entity 타입과 Value 타입의 특징 

Entity Type 이란 @Entity로 정의되는 타입으로, 데이터가 변경되더라도 **식별자에 의해 추적**이 가능하다. 반면 Value Type은 int, String과 같은 수치적인 타입으로 값이 변경되면 그저 대체될 뿐이므로 **추적이 불가능**하다. 

추적이 불가능하다는 것은 어떤 의미일까? 다음과 같은 상황을 예로 들어보자.

Member Entity Type이 있다고 가정하며, 이 엔티티 클래스는 JPA에 의해 DB의 테이블과 매핑되어있다. 이 때 어떠한 Member 객체의 값이 변경된다면, JPA는 그 변경 사항을 추적하여 커밋 시점에 그 객체의 정보를 갖고 있는 레코드를 찾아 내용을 수정할 것이다. 이는 엔티티가 식별자를 가지고 있고 그것이 DB와 매핑되어 있기 때문이다. 이러한 상황을 추적 가능하다라고 이야기한다.

반면 값 타입의 경우는 추적이 불가능한데 <!-- 이후 채우기 -->

값 타입을 공유하는 것은 변경에 있어서 side effect를 만들어낼 위험이 있어 공유하지 않는 것이 좋다. 자바의 경우 int 타입, Integer 타입 모두 side effect를 줄이기 위한 대책이 마련되어있다. int의 경우 원시타입이어서 값이 전달될 때 레퍼런스가 아닌 값 복사를 통해 전달되므로 변경 side effect가 발생하지 않으며, Integer 타입의 경우 수정이 불가능하게 되어있어 공유는 가능하지만 수정은 불가능하다.

엔티티는 비영속*Transient*, 영속*Persistent*, 분리*Detached*, 삭제*Removed*의 생명주기를 가지며, 값 타입은 자체적인 독립된 생명주기를 갖지 않고 소유하는 엔티티 타입의 생명주기에 의존한다. 

이러한 차이점 (추적 가능, 생명주기)는 이후에 나올 개념들을 익히기 위해 필요한 개념들이다.

# 분류 
Value Type은 다음과 같은 분류로 나누니다.
* **기본값 타입***Basic Value Type*
* **임베디드 타입***Embedded Type*, 복합 값 타입
* **컬렉션 값 타입***Collection Value Type*

## 기본 값 타입
String, Integer, int 와 같은 타입들을 기본 값 타입이라고 하며, 이들의 생명주기는 **엔티티에 의존**한다. 

## 임베디드 타입(복합값 타입)
기본 값 타입을 모아서 의미를 갖는 새로운 값 타입을 만든 것을 임베디드 타입이라고 한다.

![](https://i.imgur.com/Ao2DUb4.png)

### 사용방법
```java
@Embeddable
class Period {

	//기본 생성자 필수
	public Period () { }

	private LocalDateTime startDate;
	private LocalDateTime endDate 
}

@Embeddable
class Address {
	public Address() { }

	private String city;
	private String street;
	private String zipCode;
}

@Entity
class Member {
	...
	@Embedded
	private Period period;

	@Embedded
	private Address address;
}
```

임베디드 타입은 @Embeddable 애노테이션을 부착하여 생성할 수 있으며, 엔티티의 필드에 @Embedded 애노테이션을 사용하여 사용한다. 둘 중 하나만 해도 괜찮다고 하는데 직접 체크해보지는 않았으며, 의미상 둘 다 넣거나 @Embeddable만 사용하는 것이 좋아보인다.


### 특징

내부적으로 동작하는데 기본 생성자가 필요하여 생성해주어야 정상적으로 동작한다. 이후 Embedded Type은 마치 엔티티에 flat하게 선언된 것 처럼 사용할 수 있다. 메서드를 작성할 수도 있으며 재사용성을 높여주어 객체지향적인 프로그래밍을 도와준다.
![](https://i.imgur.com/4E3oGzb.png)

### 임베디드와 연관관계
임베디드 타입에서도 기존과 동일하게 연관관계 매핑 애노테이션을 사용하여 엔티티간 연관관계를 맺을 수 있다. 임베디드 타입에서 다른 임베디드 타입을 사용하는 것도 가능하다.

![](https://i.imgur.com/mTIC4bG.png)

### 속성 재정의, @AttributeOverride
한 엔티티에서 여러 임베디드 타입을 사용하는 경우 컬럼이 중복되므로, @AttributeOverride와 @AttributeOverrides를 통해서 속성을 재정의 할 수 있다.

### 임베디드 타입이 null인 경우
매핑한 컬럼의 값은 모두 null로 처리된다.

### 객체 타입의 한계
임베디드 타입은 자바 객체로 취급되므로, 여러 엔티티에서 공유될 수 있고 side effect 역시 발생할 수 있다.
![](https://i.imgur.com/QbiNBfQ.png)

이를 해결하기 위해서 임베디드 타입을 복사하여 다른 객체를 만들어 할당할 수 있지만,  원본 객체 타입 참조 값이 할당되는 것을 아예 막을 수는 없다. 따라서 side effect의 위험은 계속 존재한다. 이를 해결하기 위해서 **객체를 불변하게 만드는 방법**을 사용할 수 있다.

임베디드 타입은 값을 수정할 수 없게 만들어 부작용을 원천 차단할 수 있다. 생성자로만 값을 설정하고 수정자를 만들지 않으면 된다. 이 경우 변경이 필요할 때는 완전히 새로운 객체를 만들어야 한다.

## 값 타입 컬렉션

값 타입 컬렉션은 값 타입을 하나 이상 저장할 때 사용한다.

![](https://i.imgur.com/lcIO77E.png)

이 때 테이블 구조는 위와 같아야 한다. Relational Model에서 컬렉션은 하나의 테이블에 저장할 수 없다. 따라서 값 타입에 대한 속성들과 값 타입을 갖고 있을 엔티티의 ID를 모두 PK로 설정한 테이블을 만들어 값 타입 컬렉션을 구현할 수 있다.

### JPA에서 사용 방법
```java
class Member {
	...
	@ElementCollection
	@CollectionTable(
		name = "FAVORITE_FOOD",
		joinColumns = @JoinColumn(name = "MEMBER_ID")
	)
	@Column(name = "FOOD_ID")
	private Set<String> favoriteFoods = new ArrayList<>();

	@ElementCollection
	@CollectionTable(
		name = "ADDRESS",
		joinColumns = @JoinColumn(name = "MEMBER_ID")
	)
	private List<Address> addressHistory = new ArrayList<>();
}
```

* **@ElementCollection** : 이 필드가 값 타입 컬렉션임을 지정한다.
* **@CollectionTable** : 값 타입 컬렉션을 저장할 테이블에 관련된 정보를 작성한다.
	* name : 테이블의 이름
	* joinColumn : 원본 엔티티 테이블과 조인할 FK 지정

```java
//저장 예제
Member member = new Member();
member.getFavoriteFoods().add("불고기");
member.getFavoriteFoods().add("비빔밥");

member.getAddressHistory().add(new Address(...));
member.getAddressHistory().add(new Address(...));

em.persist(member);
```

위와 같이 저장해주면 DB의 별도 테이블에 값타입 컬렉션이 들어간다. 값 타입 컬렉션에 해당하는 테이블은 모든 컬럼이 PK로 묶여있으므로 null을 저장할 수 없다. 

```java
//조회 예제

Member member = em.find(1L, Member.class);

member.getAddressHistory(); //이 메서드가 호출될 때 쿼리 수행
```

값 타입 컬렉션의 조회는 지연 로딩 전략을 사용한다.

```java
// 수정 예제

//불고기를 삼겹살로 변경하고 싶다면
member.getFavoriteFoods().remove(0);
member.getFavoriteFoods().add("삼겹살");

```

Favorite Foods의 경우 변화 불가능한 타입인 String을 사용하므로, 값을 수정하려면 불고기를 제거한 뒤 삼겹살을 추가하는 방법으로 코드를 작성해주어야 한다.

이 경우 쿼리는 어떻게 나갈까? 불고기에 해당하는 레코드를 삼겹살로 update하는 쿼리가 나갈까? 아니면 불고기를 삭제하는 쿼리가 나간 뒤 삼겹살을 추가하는 코드가 나갈까? 사실 둘다 아니다.

값 타입은 별도의 ID가 없기 때문에 추적할 수 없다. 추적이 안된다는 의미는 위와 같은 동작이 발생했을 때 DB와 Application의 입장에서 어떤 것이 변경됐는지 알 수 없다는 것이다. JPA는 이 문제를 테이블의 내용을 모두 지운 뒤 현재 favoriteFoods에 저장되어있는 값을 새롭게 insert 하는 것으로 해결한다.
