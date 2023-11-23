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