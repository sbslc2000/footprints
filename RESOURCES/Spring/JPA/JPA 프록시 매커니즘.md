# 논의점

하나의 엔티티를 조회할 때 그것과 연관되어있는 다른 엔티티도 모두 함께 조회해야할까?
```java
Member findMember = em.find(Member.class, 1L);
System.out.pritnln("Username: "+member.getUsername());
//User.team 은 사용되지 않음
```

때에 따라서는 연관관계에 있는 객체들은 사용되지 않을 수도 있다. 하지만 모든 상황에서 이러한 객체들을 함께 조회한다면 성능 최적화가 이루어지지 않을 수 있다. 

# 프록시를 통한 해결
Hibernate는 위와 같은 문제를 프록시를 통해 해겷한다. 이 문서에서는 JPA에서 프록시를 어떻게 다루는지에 대해서 설명하고, 위 문제를 해결하는 내용은 [[지연로딩과 즉시로딩]] 에서 다루기로 한다.

## em.find() vs em.getReference()

em.find()는 DB에서 엔티티의 값을 조회하여 바인딩한 결과인 엔티티 클래스를 반환한다. 따라서 select 쿼리는 find를 호출하는 순간 발생한다.

반면 em.getReference()는 엔티티의 값을 즉시 조회하지 않고, 조회할 준비만 되어있는 프록시 객체를 반환한다. 따라서 select 쿼리는 프록시 객체를 통해 값을 조회하는 순간에 발생한다.

```java
Member m1 = em.find(Member.class, 1L); //쿼리 호출
Member m2 = em.getReference(Member.class, 2L); //쿼리 호출 x

System.out.println("m1.getClass() : " + m1.getClass()); //Member
System.out.println("m2.getClass() : " + m2.getClass()); //Member$HibernateProxy

m1.getUsername(); //쿼리 호출 x
m2.getUsername(); //이 시점에서 쿼리 호출
```

## 프록시 객체 초기화

프록시 객체가 뒤늦게 필요한 값을 DB에서 가져와 영속성 컨텍스트에 저장하는 과정을 초기화라고 한다.
![](https://i.imgur.com/svTCjkD.png)
em.getReference()로 생성된 프록시 객체에 값 조회를 요청했을 때, 프록시 객체는 이 값이 존재하는지 판단하고 만약 없다면 DB를 통해 Entity를 가져온다. 이후 프록시의 target을 통해 객체의 값을 반환한다.
따라서 한 번 요청이 수행되고 나면 이후에는 별도의 쿼리 전달 없이 값을 반환한다.

# 주의할 점

1. **타입 체크**
프록시 객체와 원본 객체를 == 연산을 통해 타입을 비교할 때 문제가 발생할 수 있다.
```java
Member findMember = em.find(Member.class, 1L);
Member proxyMember = em.getReference(Member.class, 2L);

System.out.println("type check : "+findMember == proxyMember); //false
```
findMember은 Member 클래스이며, proxyMember은 Member 클래스를 상속한 Member$HibernateProxy 클래스이다. 둘은 서로 클래스 타입 정보가 달라 위와 같은 경우 false를 반환한다. 이 점을 유의하지 않는다면 의도치 않은 동작이 수행될 수 있다.

2. **em.getReference()는 항상 프록시 객체를 반환하는 것은 아니다.**
만약 1번 ID에 해당하는 엔티티가 영속성 컨텍스트에 의해 관리중이라면, `em.getReference(Member.class, 1L)`을 통해 받은 객체는 프록시 객체가 아닌 원본 객체이다. 
프록시 객체는 조회를 지연한다는데에서 의미를 가지는데, 이미 조회 데이터가 있는 시점에서는 성능상의 이점을 챙길 수 없기 때문이다.

```java
Member findMember = em.find(Member.class, 1L); //원본
Member referenceMember = em.getReference(Member.class, 1L); //역시 원본

System.out.println("type check : "+findMember == referenceMember); //true
```

3. **em.find() 역시 항상 원본 객체를 반환하지는 않는다.**

JPA spec에는 하나의 트랜잭션 내에서 조회된 엔티티들은 == 연산을 통해 동일성을 판단할 수 있어야 한다는 조건이 있다. 이에 따라 em.find()는 프록시에 감싸진 채로 target에 완전한 엔티티정보를 담아 반환되기도 한다.

```java
Member referenceMember = em.getReference(Member.class, 1L); //프록시
Member findMember = em.find(Member.class, 1L); //프록시 내부에 완전한 엔티티 담김

System.out.println("type check : "+findMember == referenceMember); //true
System.out.println("findMember.getClass() : "+findMember.getClass());//Member$HibernateProxy
```

이 때 referenceMember와 findMember가 가리키고 있는 프록시 객체는 동일한 객체이다.

4. **준영속 상태일 때, 프록시 객체 초기화를 진행하면 예외가 발생한다.**

객체가 준영속 상태라면 영속성 컨텍스트에 의해 관리되지 않으므로, 프록시를 초기화할 때에 LazyInitializationException이 발생한다.

```java
Member referenceMember = em.getReference(Member.class, 1L); //프록시

em.detach(referenceMember);
//em.clear() 이것도 마찬가지로 referenceMember를 준영속 상태로 만듦

referenceMember.getUsername();// LazyInitializationException 발생!
```

# 프록시 관련 Utility 메서드

* **프록시 인스턴스의 초기화 여부 확인**
```java
emf.getPersistenceUnitUtil().isLoaded(referenceMember); // true or false
```
* ** 프록시 강제 초기화**
프록시의 target에 DB로부터 Entity를 가져온다. 이는 [[Hibernate]]에서 제공하는 메서드로, JPA에서는 강제 초기화 기능을 제공하지 않는다.
```java
Hibernate.initialize(referenceMember); 
```


### 더 보기
