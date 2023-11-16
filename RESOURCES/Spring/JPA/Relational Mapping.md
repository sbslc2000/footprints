#  Terminologies
- **다중성** : 레코드간 연관을 맺을 수 있는 상대 테이블 레코드의 개수
    - 다대일(혹은 N:1)
    - 일대일(혹은 1:1)
    - 다대다(혹은 N:M)
소프트웨어 공학에서 다루는 객체 간 multiplicity와 동일한 개념입니다.
- **[[단방향, 양방향]]**
테이블에서는 외래 키가 어디에 있는지에 상관없이 Join을 통해 양방향으로 쿼리가 가능하므로 방향성이라는 개념이 없습니다. 반면 객체는 객체 참조를 통해 연관관계를 형성하므로 방향성이 존재합니다.
* [[연관관계의 주인]]



# 다대일 \[N : 1]

## 다대일 단방향

![](https://i.imgur.com/cnUKiLS.png)
```java
class Member {
	...
	@ManyToOne
	@JoinColumn(name="TEAM_ID")
	Team team;
}
```

## 다대일 양방향

![](https://i.imgur.com/ry8oaW5.png)
```java
class Member {
	@ManyToOne
	@JoinColumn(name="TEAM_ID")
	Team team;
}

class Team {
	...
	@OneToMany(mappedBy="team") //반대측 필드 이름
	private List<Member> members = new ArrayList<>();
}
```

양방향 매핑을 한다 하더라도 DB 테이블에는 영향을 주지 않는다.

### JoinColumn과 mappedBy

JoinColumn과 mappedBy는 연관관계의 주인을 지정해주기 위해 사용한다. 연관관계의 주인 측에서는 @JoinColumn을 통해 FK를 명시해줘야하며, 주인이 아닌 측에서는 mappedBy를 통해 매핑되는 반대측에 연결될 필드를 지정해주어야 한다.

JoinColumn에 컬럼명을 지정해준다고 해서 항상 해당 엔티티 테이블에 FK가 생기는 것은 아니라는 것에 주의해야한다. 데이터베이스 다대일 관계에서 항상 FK는 다 측에 생성된다.

mappedBy를 사용한 필드는 오직 읽기 전용으로 사용되며, 이곳에서 발생한 변경은 FK에 반영되지 않는다.

# 일대다 \[1:N]

다 측이 아닌 일 측을 연관관계의 주인으로 설정하려면 어떻게 해야할까?

## 일대다 단방향
![](https://i.imgur.com/71B37uv.png)

DB 설계 상 FK는 다 측에 고정될 수 밖에 없다. 하지만 객체에서는 일 측에 연관관계를 설정해줄 수 있다. 이런 경우 연관관계를 관리하는 측이 객체 모델과 테이블 모델에서 다른 특별한 케이스가 된다.
```java
class Team {
	@OneToMany
	@JoinColumn(name="TEAM_ID")
	private List<Member> members = new ArrayList<>();
}
```
이 경우 JoinColumn을 TEAM_ID라는 이름으로 하지만, 이 키는 TEAM 테이블이 아닌 MEMBER 테이블 측에 저장된다.

```java
//main.java
Member member = new Member("Pete");
em.persist(member);

Team team = new Team("teamA");

team.getMembers().add(member);
em.persist(team);
```
이제는 일 측에서 연관관계를 수정하더라도 데이터베이스에 반영이 된다. 하지만 이런 케이스에서는 여러 [부작용들이 발생](연관관계의%20주인#연관관계의%20주인)한다.

### JoinColumn을 사용하지 않는다면

@OneToMany 측을 연관관계의 주인으로 설정하는 경우 @JoinColumn을 사용하지 않는다면 Join Table 방식을 도입하여 DB 레벨의 연관관계를 형성한다. 이 경우 추가적인 테이블이 생겨서 성능상의 단점이 생기며 운영이 어려워지므로 JoinColumn 애노테이션을 꼭 사용하는 것이 좋다. (사실 일대다는 안쓰는 것이 제일 좋다.) 

## 일대다 양방향

![](https://i.imgur.com/387uEPv.png)

```java

class Member {
	...
	@ManyToOne
	@JoinColumn(name = "TEAM_ID", insertable = false, updatable = false)

}
class Team {
	@OneToMany
	@JoinColumn(name="TEAM_ID")
	private List<Member> members = new ArrayList<>();
}
```

사실 일대다 양방향은 JPA에서 지원하는 스펙은 아니지만, 연관관계의 주인을 설정하는 것처럼 @ManyToOne에 @JoinColumn을 작성해주고 읽기 전용 속성을 넣어주면 마치 양방향 일대다처럼 사용할 수 있다.

# 일대일 \[1:1]

일대일 관계는 그 반대도 일대일이며, 주 테이블이나 대상 테이블 중에 외래키를 놓을 테이블을 선택할 수 있다. 일대일 관계에서는 무결성을 위해 외래 키에 UNIQUE 제약조건이 있어야 한다.

## 일대일 : 주 테이블에 외래 키 단방향

![](https://i.imgur.com/7YtpRvx.png)

```java
@Entity 
class Member {
	...
	@OneToOne
	@JoinColumn(name="LOCKER_ID")
	private Locker locker;
}

@Entity
class Locker {
	@Id @GeneratedValue
	private Long id;
	
}
```

## 일대일 : 주 테이블에 외래키 양방향
```java
@Entity 
class Member {
	...
	@OneToOne
	@JoinColumn(name="LOCKER_ID")
	private Locker locker;
}

@Entity
class Locker {
	@Id @GeneratedValue
	private Long id;

	@OneToOne(mappedBy="locker")
	private Member member;	
}
```

## 일대일 : 대상 테이블에 외래 키 단방향

![](https://i.imgur.com/9qxmMOy.png)

이러한 구조는 지원하지 않는다. 대신 양방향은 가능하다.

## 일대일 : 대상 테이블에 외래 키 양방향

![](https://i.imgur.com/gAqRTCM.png)

사실상 주 테이블의 외래키 양방향과 동일하다.

### 한계
프록시 기능의 한계로 지연 로딩으로 해놓아도 항상 즉시 로딩이 적용된다.

# 다대다 \[M:N]

객체는 컬렉션을 사용해서 다대다 관계를 표현할 수 있다. 하지만 테이블에서는 다대다 관계가 불가능하여 다대다 관계를 다대일과 일대다로 분리해주어야 한다.

```java
class Memeber {
	...
	@ManyToMany
	@JoinTable(name="MEMBER_PRODUCT")
	private List<Product> products = new ArrayList<>();
}
```

다대다 매핑은 편리해보이지만 실무에서 사용하기 어렵다. 다대다 매핑에서는 구조상 연관관계에 추가되는 정보들을 저장할 수 없다. 또한 애플리케이션 코드 입장에서 테이블이 숨겨져 있기 때문에 예상치 못한 쿼리가 나갈 가능성이 있다.