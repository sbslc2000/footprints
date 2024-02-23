# @DataJpaTest
@DataJpaTest 는 스프링부트의 기능으로, Data Access Layer만 테스트할 수 있게 해준다. 이 때에는 다른 빈들은 등록이 안되며, Repository와 관련된 빈들만 생성이 된다.

@DataJpaTest 애노테이션은 [[@Transactional]] 애노테이션의 합성 애노테이션이다. 따라서 매번 롤백이 수행되므로 Insert 쿼리가 수행되지 않는 것이 기본이다 (Hibernate의 쿼리 지연). 이러한 상황은 테스트를 어렵게 만들 수 있다.

이 경우 Insert 쿼리를 보고 싶다면 테스트 메서드에 @Rollback(false) 애노테이션을 사용해줄 수 있다.

## 주의사항
테스트용 환경변수에 MySQL Datasource를 등록해 놓았더라도 @DataJpaTest는 기본적으로 메모리 h2를 사용하여 수행된다. 아마 h2 드라이버가 build.gradle에 있을 때에만 해당할 듯 하다.