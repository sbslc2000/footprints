# REST 
REST(Representational State Transfer) 아키텍처 스타일이 로이 필딩의 논문에서 제안된 분산 하이퍼미디어 시스템을 위한 아키텍처 스타일이다.

## 개념

### Resource
REST에서 정보(information)을 추상화한 것의 핵심은 자원(resource)이다. 이름을 붙일 수 있는 모든 정보는 자원이 될 수 있다.

### Representation
REST 컴포넌트들은 표현(representation)을 이용하여 현재, 또는 특정시점의 리소스의 상태(status)를 파악하고, 이 표현을 전송함으로써 리소스에 대한 작업을 수행한다.

### State Transfer
애플리케이션의 상태는 변화한다. 이 상태 전이는 서버가 제공하는 하이퍼 미디어 표현을 통해 이루어져야 한다.

## 제약 조건

### Client-server 
REST는 클라이언트-서버 아키텍처를 따른다. 이는 관심사의 분리(separation of concerns) 효과를 낳는다.

데이터의 저장에 대한 관심사와 사용자 인터페이스에 대한 관심사를 분리함으로써, 클라이언트 측면에서는 여러 플랫폼에 대한 사용자 인터페이스의 이식성(portability)를 개선시키고, 서버 측면에서는 서버측의 구성요소를 단순화함으로써 확장성(scalability)를 개선시킨다. 이를 통해 각각의 요소는 독립적으로 발전할 수 있다.

### Stateless
REST에서 클라이언트와 서버간의 통신이 본질적으로 무상태성을 가진다. 클라이언트가 서버로 보내는 요청에는 해당 요청을 이해하는 데에 필요한 모든 정보가 포함되어있어야 하고, 서버측에 저장된 정보를 이용할 수 없다.

이 제약 조건은 다음 성질을 유도한다.

* 가시성 : 요청에 대한 특징을 파악하기 위해 해당 요청 외에 다른 것들을 고려하지 않아도 된다.
* 신뢰성 : 부분적인 장애를 복구하기 위해서 고려해야할 것들이 많지 않기 때문에 복구가 용이해진다.
* 확장성 : 서버측은 요청에 대한 정보를 기억할 필요가 없기에 구현이 더욱 쉬워진다.

### Cache
Cache 제약 조건은 응답 데이터가 cacheable한지, non-cacheable한지 암시적 혹은 명시적으로 지정되어야 한다는 것이다. 만약 응답이 cacheable하다면, 클라이언트 캐시에 동일한 요청에 대해 해당 응답데이터를 나중에 재사용할 수 있는 권한이 부여된다.

### Uniform Interface
REST 아키텍처 스타일이 다른 네트워크 기반 아키텍처 스타일과 구분되는 핵심적인 특징은 REST를 이루는 컴포넌트 간의 uniform interface이다. 컴포넌트간의 인터페이스에 소프트웨어 공학의 원칙인 일반성(generality)를 적용함으로써, 전체적인 시스템 아키텍처는 단순화되고 상호작용의 가시성은 향상된다.

Uniform Interface에 대한 제약조건은 4가지로 정의된다.

1. identification of resources
2. manipulation of resources through representations
3. self-descriptive message
4. hypermedia as the engine of application state

### Layered System
REST는 Layered System 스타일을 통해 각 컴포넌트에 직접 접하고 있는 계층 너머의 컴포넌트를 볼 수 없도록 컴포넌트의 동작을 제한함으로써 아키텍처가 계층적인 층을 구성하도록 한다.

예를 들어, 클라이언트는 자신이 직접 서버와 통신하는지, 아니면 프록시, 게이트웨이, 로드밸런서와 같은 중간 계층을 거쳐서 통신하는지 몰라야 한다.

### Code on demand
Code on demand란 서버가 클라이언트에게 실행 가능한 코드(ex. javascript)를 내려복내고, 클라이언트가 이를 실행할 수 있도록 허용하는 방식이다. 즉, 서버가 클라이언트의 기능을 확장하거나 커스터마이징 할 수 있도록 실행 코드를 전달하는 것을 의미한다.