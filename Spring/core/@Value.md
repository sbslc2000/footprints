---
상위 링크: "[[Spring Core]]"
---
# @Value
@Value를 사용하면 해당 필드 혹은 파라미터에 Environment로부터 값을 꺼내 주입해준다. 또한 `:`을 사용하여 설정 데이터가 없는 경우 기본값을 제공해줄 수 있다.

```java
@Value("${my.datasource.url:local.db}") 
```

이 방법은 편리하지만, @Value를 통해 외부 설정 정보의 키 값을 입력받고 주입받아야하는 부분이 번거로울 수 있다. 그리고 당연하지만, 스프링이 관리하는 빈이 아니라면 이 값들을 주입받을 수 없다.

## 예시

```java
@Slf4j  
@Configuration  
public class MyDataSourceValueConfig {  
  
    @Value("${my.datasource.url}")  
    private String url;  
  
    @Value("${my.datasource.username}")  
    private String username;  
  
    @Value("${my.datasource.password}")  
    private String password;  
  
    @Value("${my.datasource.etc.max-connection}")  
    private int maxConnection;  
  
    @Value("${my.datasource.etc.timeout}")  
    private Duration timeout;  
  
    @Value("${my.datasource.etc.options}")  
    private List<String> options;  
  
    @Bean  
    public MyDataSource myDataSource1() {  
        return new MyDataSource(url, username, password, maxConnection, timeout, options);  
    }  
  
    @Bean  
    public MyDataSource myDataSource2(  
            @Value("${my.datasource.url}") String url,  
            @Value("${my.datasource.username}") String username,  
            @Value("${my.datasource.password}") String password,  
            @Value("${my.datasource.etc.max-connection}") int maxConnection,  
            @Value("${my.datasource.etc.timeout}") Duration timeout,  
            @Value("${my.datasource.etc.options}") List<String> options) {  
        return new MyDataSource(url, username, password, maxConnection, timeout, options);  
    }  
}
```