---
상위 링크: "[[Spring Boot]]"
---
# @ConfigurationProperties
### Type-safe Configuration Properties
스프링은 외부 설정의 묶음 정보를 객체로 변환하는 기능을 제공한다. 이를 **'타입 안전한 설정 속성\'** 이라 한다.
객체를 사용하면 타입을 사용할 수 있다. 따라서 실수로 잘못된 타입이 들어오는 문제도 방지할 수 있고, 객체를 통해서 활용할 수 있는 요소가 많아진다. 

## @ConfigurationProperties
사용하고자 하는 프로퍼티들이 존재하는 클래스를 생성한뒤, 엘리먼트에 공통 외부 설정 정보의 기본 주소를 작성한다. 이후 이 클래스가 이후 방법에 의해 등록되면 기본 주소와 변수명을 조합한 데이터를 찾아 값을 저장한다.

기본 주입 방식은 자바 빈 프로퍼티 방식인 getter, setter 방식이므로 @Data 혹은 게터 세터 코드를 작성해주어야 한다.
```java
@Data  
@ConfigurationProperties("my.datasource")  
public class MyDataSourcePropertiesV1 {  
  
    private String url;  
    private String username;  
    private String password;  
    private Etc etc = new Etc();  
  
    @Data  
    public static class Etc {  
        private int maxConnection;  
        private Duration timeout;  
        private List<String> options = new ArrayList<>();  
    }  
}
```

## @EnableConfigurationProperties
위 객체에 데이터를 주입하기 위해서는 다음과 같은 설정을 해주어야 한다.
```java
@EnableConfigurationProperties(MyDataSourcePropertiesV1.class)  
public class MyDataSourceConfigV1 {  
  
    private final MyDataSourcePropertiesV1 properties;  
  
    public MyDataSourceConfigV1(MyDataSourcePropertiesV1 properties) {  
        this.properties = properties;  
    }  
  
}
```
@EnableConfigurationProperties()를 통해 스프링에게 사용할 @ConfigurationProperties 클래스를 지정해줄 수 있다. 이 경우 해당 클래스는 스프링 빈으로 등록되고, 필요한 곳에서 주입 받아 사용할 수 있다.

이 방법을 사용한다면 타입 안전을 보장받을 수 있다. 만약 int가 들어와야하는 maxConnection에 abc와 같은 문자열이 입력된다면 스프링 초기화 과정에서 다음과 같은 에러가 나타난다.
```
 Failed to bind properties under 'my.datasource.etc.max-connection' to int:

     Property: my.datasource.etc.max-connection
     Value: "abc"
     Origin: class path resource [application.properties] - 4:34
     Reason: failed to convert java.lang.String to int ...
```

## @ConfigurationPropertiesScan
만약 특정 패키지의 ConfigurationProperties 클래스를 모두 등록하고 싶다면 @ConfigurationPropertiesScan 애노테이션을 통해 자동등록을 할 수 있다.


## 생성자 방식의 객체 생성
다음과 같은 방식으로 생성자를 통해 값을 주입하여 프로그램의 동작 중에 값이 변경되지 않게 만들 수 있다.
```java
@Getter  
@ConfigurationProperties("my.datasource")  
public class MyDataSourcePropertiesV2 {  
  
    private final String url;  
    private final String username;  
    private final String password;  
    private final Etc etc;  
  
    public MyDataSourcePropertiesV2(String url, String username, String password, @DefaultValue Etc etc) {  
        this.url = url;  
        this.username = username;  
        this.password = password;  
        this.etc = etc;  
    }  
  
    @Getter  
    public static class Etc {  
        private final int maxConnection;  
        private final Duration timeout;  
        private final List<String> options;  
  
        public Etc(int maxConnection, Duration timeout, @DefaultValue("DEFAULT") List<String> options) {  
            this.maxConnection = maxConnection;  
            this.timeout = timeout;  
            this.options = options;  
        }  
    }  
}
```

생성자를 만들어 두면 생성자를 통해서 설정 정보를 주입한다. 이 때, 외부 설정을 통해 값을 찾을 수 없는 경우라면 @DefaultValue 애노테이션에서 제공된 기본값을 사용한다.

만약 @DefaultValue 애노테이션에 기본값이 제공되어 있다면, 외부설정의 값이 들어오지 않더라도 예외를 발생시키지 않는다.

> [!info] @ConstructorBinding
> 스프링부트 3.0 이전에는 생성자 바인딩을 할 때에 @ConstructorBinding 애노테이션을 필수로 사용해야 했다.
> 스프링부트 3.0 부터는 생성자가 하나일 때는 생략할 수 있다. 생성자가 둘 이상인 경우에는 바인딩에 사용할 생성자에 @ConstructorBinding을 부착해주어야 한다.

## 검증
만약 외부 설정 값을 애플리케이션 내에서 검증하고 싶다면 어떻게 해야할까?
@ConfigurationProperties는 자바 객체이기 때문에 스프링이 자바 빈 검증기를 사용할 수 있도록 지원한다.

```groovy
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

```java
@Getter  
@ConfigurationProperties("my.datasource")  
@Validated  
public class MyDataSourcePropertiesV3 {  
  
    @NotEmpty  
    private final String url;  
    @NotEmpty  
    private final String username;  
    @NotEmpty  
    private final String password;  
  
    private final Etc etc;  
  
    public MyDataSourcePropertiesV3(String url, String username, String password, Etc etc) {  
        this.url = url;  
        this.username = username;  
        this.password = password;  
        this.etc = etc;  
    }  
  
    @Getter  
    public static class Etc {  
        @Min(1)  
        @Max(999)  
        private final int maxConnection;  
  
        @DurationMin(seconds = 1)  
        @DurationMax(seconds = 60)  
        private final Duration timeout;  
        private final List<String> options;  
  
        public Etc(int maxConnection, Duration timeout, List<String> options) {  
            this.maxConnection = maxConnection;  
            this.timeout = timeout;  
            this.options = options;  
        }  
    }  
}
```

이를 통해 애플리케이션 로딩 시점에 원하지 않는 값으로 들어온 외부 설정을 검증하고 프로그램의 동작을 거부시킬 수 있다.