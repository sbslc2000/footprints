---
상위 개념: "[[Spring Data COMMON]]"
---
# Custom Repository
쿼리 메서드로 해결이 되지 않는 경우 직접 코딩으로 구현 가능하다.
스프링 데이터 레포지토리 인터페이스에 기능을 추가할 수 있으며, 기본 기능을 덮어쓰기하는 것도 가능하다.

스프링 데이터 레포지토리 인터페이스에 추가할 행동의 목록을 담은 인터페이스를 작성한다. 이 인터페이스에는 어떠한 네이밍 제약도 존재하지 않는다.
```java
public interface  PostCustomRepository {  
  
    List<Post> findMyPost();  
}
```

이후 해당 인터페이스를 구현하여 추가할 기능을 작성한다. 이 때에는 미리 약속된 네이밍 규칙이 있어서 이를 따라야한다. 기본값은 인터페이스 뒤에 Impl을 붙이는 것이다.
```java
public class PostCustomRepositoryImpl implements PostCustomRepository {  
  
    private final EntityManager em;  
  
    @Override  
    public List<Post> findMyPost() {  
        log.info("custom findMyPost");  
        return em.createQuery("select p from Post p", Post.class)  
                .getResultList();  
    }  
}
```

JpaRepository를 상속하는 인터페이스에 작성한 custom 인터페이스를 상속하면, Spring Data Common이 상속한 인터페이스의 구현체를 찾아 상속시킨다.
```java
public interface PostRepository extends JpaRepository<Post, Long>, PostCustomRepository {  
}
```

이를 통해 JpaRepository 빈에서 기본적으로 제공하는 기능 뿐만 아니라 직접 개발한 코드도 수행하게 만들 수 있다. 또한 기본적으로 제공하는 메서드를 오버라이드해서 **기본 기능을 덮어씌우는 것**도 가능하다.


Impl이 붙어야 하는 컨벤션을 변경하고 싶다면 @EnableJpaRepositories의 **repositoryImplementationPostfix** 속성을 변경하여 접미어를 원하는대로 지정할 수 있다.
```java
@SpringBootApplication
@EnableJpaRepositories(repositoryImplementationPostfix="Default")
public class MyApplication { 
...
}
```


# 기본 레포지토리 커스터마이징
모든 레포지토리에 공통적으로 적용시키고 싶은 기능이 있다면 어떻게 해야할까?

1. JpaRepository를 상속받는 인터페이스를 정의한다. 
이곳에 공통적으로 추가하고자 할 행동 목록을 작성한다. 이 인터페이스는 빈으로 등록되지 않아야 하므로 @NoRepositoryBean을 작성해주어야 한다.
```java
@NoRepositoryBean  
public interface MyRepository<T, ID extends Serializable> extends JpaRepository<T, ID> {  
  
    boolean contains(T Entity);  
}
```

2. 직접 만든 인터페이스를 구현하는 클래스를 작성한다.
JpaRepository의 기본 구현체인 SimpleJpaRepository를 상속하여 기능을 받아와야하며, 새로운 공통기능을 담고 있는 MyRepository를 implement하여 수행될 내용을 작성해준다.
```java
public class MyRepositoryImpl<T, ID extends Serializable> extends SimpleJpaRepository<T, ID> implements MyRepository<T, ID> {  
  
    private EntityManager entityManager;  
    
    public MyRepositoryImpl(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager) {  
        super(entityInformation, entityManager);  
        this.entityManager = entityManager;  
    }  
  
    @Override  
    public boolean contains(T entity) {  
        return entityManager.contains(entity);  
    }  
}
```

3.  @EnableJpaRepository에 구현체의 클래스를 등록해준다.
repositoryBaseClass 속성은 repository proxy를 설정하기 위한 기반 클래스를 지정해주며, 기본적으로는 아무런 추가적인 구현사항이 없는 DefaultRepositoryBaseClass로 설정이 되어있다. 
```java
@SpringBootApplication  
@EnableJpaRepositories(repositoryBaseClass = MyRepositoryImpl.class)  
public class Datajpa2Application {  
  
    public static void main(String[] args) {  
        SpringApplication.run(Datajpa2Application.class, args);  
    }  
  
}
```

```java
public final class DefaultRepositoryBaseClass {  
  
    private DefaultRepositoryBaseClass() {}  
}
```

4. 사용하고자 하는 레포지토리의 인터페이스를 변경한다.
```java
public interface PostRepository extends MyRepository<Post,Long> {  
}
```

이후 MyRepository를 상속한 모든 레포지토리 빈은 직접 구현한 메서드를 사용할 수 있게 된다.