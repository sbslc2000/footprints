# Repository

![](https://i.imgur.com/kuIu6sB.png)

Spring Data COMMON의 리포지토리는 위와 같은 계층구조를 갖는다.

가장 상위의 Repository 인터페이스는 실질적인 기능을 하지는 않고, 이것을 상속한 클래스가 레포지토리 역할을 한다는 의미만 제공한다. 이러한 형태로 인터페이스를 사용하는 것을 Marker Interface라고도 부른다.

CrudRepository와 그 하위의 인터페이스들은 @NoRepositoryBean이라는 애노테이션이 부착되어있다. @NoRepositoryBean 애노테이션은 해당 클래스의 빈이 사용자에 의해서 직접 등록되지 않도록 막아주는 역할을 한다.

Repository 인터페이스와 다르게 CrudRepository부터는 기능을 제공한다. `save`, `saveAll`, `findById`, `existsById`, `findAll`, `findAllById`, `count`, `deleteById`, `delete` 와 같은 CRUD 기능을 제공한다.

PagingAndSortingRepository에서는 findAll의 파라미터로 Pageable의 구현체와 Sort를 담아 전송할 수 있다.

#todo <!-- 아래 문장은 추후 Pageable을 설명하는 문서로 이관 예정 --> 
Page 객체의 size 속성은 요청 시 한 페이지에 담을 개수를 의미하며, numberOfElements는 실제로 해당 페이지에 들어온 요소의 개수를 의미한다.



