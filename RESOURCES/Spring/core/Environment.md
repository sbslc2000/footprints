---
상위 링크: "[[Spring Core]]"
---
# 외부 설정 - 스프링 통합
외부 설정 값 (OS 변수, 자바 시스템 속성, 커맨드 라인 인수)는 모두 외부 설정을 key-value 형태로 받는다. 그런데 외부 설정을 읽어서 사용하는 개발자 입장에서는 읽어야 하는 값들에 따라 읽는 방법이 다 다르다는 불편함을 겪게 된다. 상황에 따라서는 변화된 정책에 따라 변수를 읽어들이는 코드를 변경해야하는 문제도 생길 수 있다.

스프링은 이러한 문제를 해결하기 위해 **Environment**와 **PropertySource**를 통해 외부 환경변수를 추상화하여 제공한다.
![](https://i.imgur.com/Nif0h4Q.png)

## PropertySource
스프링은 PropertySource라는 추상 클래스를 제공하고, 각각의 외부 설정을 조회하는 XXXPropertySource 구현체를 만들어 두었다.

스프링은 로딩 시점에 필요한 PropertySource 들을 생성하고, Environment에서 사용할 수 있게 연결해둔다.

## Environment
Environment를 통해서 특정 외부 설정에 종속되지 않고, 일관성 있게 key-value 형식의 외부 설정 데이터를 조회할 수 있게 해준다.
* environment.getProperty(key) 를 통해서 값을 조회할 수 있다.
* Environment는 내부의 여러 과정을 거쳐서 PropertySource 들에 접근한다.
* 같은 값이 있을 경우를 대비해서 스프링은 미리 우선순위를 정해두었다.
모든 외부 설정은 이제 Environment를 통해 조회하면 된다.

Environment 역시 빈으로 등록되어있어 원하는 곳에 어디든 주입받아 사용하면 된다.

## 우선순위
만약 서로 다른 외부 설정 소스에서 값이 중복되면 어떻게 될까? Environment는 서로 다른 소스 간 우선순위를 두어 관리하도록 했다.

우선순위는 다음과 같은 규칙만을 따른다.
* **더 유연한 것이 우선권을 가진다.** 
	* 변경하기 어려운 파일보다 실행시 원하는 값을 줄 수 있는 자바 시스템 속성이 더 우선권을 가진다.
* **범위가 넓은 것 보다 좁은 것이 우선권을 가진다.** 
	* 자바 시스템 속성은 JVM 안에서 모두 접근할 수 있고, 커맨드 라인 옵션 인수는 main의 args를 통해 들어오기 때문에 접근 범위가 더 좁다. 따라서 커맨드 라인 옵션 인수가 더 우선순위를 갖는다.

## 사용법
```yml
//application.properties
my.datasource.url=local.db.com  
my.datasource.username=local  
my.datasource.password=local_pw  
my.datasource.etc.max-connection=1  
my.datasource.etc.timeout=3500ms  
my.datasource.etc.options=CACHE,ADMIN
```

```java
@Slf4j  
@Configuration  
public class MyDataSourceEnvConfig {  
  
    private final Environment env;  
  
    public MyDataSourceEnvConfig(Environment env) {  
        this.env = env;  
    }  
  
    @Bean  
    public MyDataSource myDataSource() {  
        String url = env.getProperty("my.datasource.url");  
        String username = env.getProperty("my.datasource.username");  
        String password = env.getProperty("my.datasource.password");  
        int maxConnection = env.getProperty("my.datasource.etc.max-connection", Integer.class);  
        Duration timeout = env.getProperty("my.datasource.etc.timeout", Duration.class);  
        List<String> options = env.getProperty("my.datasource.etc.options", List.class);  
  
        return new MyDataSource(url, username, password, maxConnection, timeout, options);  
    }  
}
```

스프링이 제공하는 Environment는 다양한 타입들에 대한 기본적인 타입 변환을 제공한다. (스프링 내부 변환기가 작동한다)

스프링 속성 변환기 공식 문서
[Core Features](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.typesafe-configuration-properties.conversion)
