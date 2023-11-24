![[Pasted image 20230921184801.png]]
1. JPA는 Persistence라는 클래스를 통해 persistence.xml의 정보를 읽은 뒤, 그 정보를 통해 EntityManagerFactory를 생성함
2. 이후 필요할 때마다 EntityManagerFactory로부터 EntityManager를 만들어서 사용

## 주의
* EntityManagerFactory는 하나만 생성해서 애플리케이션 전체에서 공유
* EntityManager는 [쓰레드](Thread.md)간에 공유하지 않는다.
* JPA의 모든 데이터 변경은 트랜잭션 안에서 실행

# 스프링 빈에 EMF 주입받기

@PersistenceUnit을 사용하면 EMF를 주입받을 수 있다.
```java
@PersistenceUnit  
private EntityManagerFactory emf;
```

@PersistenceContext를 사용하면 EM을 주입받을 수 있다.
```java
@PersistenceContext
private EntityManager em;
```