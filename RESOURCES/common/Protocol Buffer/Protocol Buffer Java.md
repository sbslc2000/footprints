# Protocol Buffer Java Tutorial
해당 내용은 Protocol Buffer 2 버전을 기준으로 작성되었습니다.

## Protocol Buffer 설치
Protocol Buffer 소스코드는 다음 주소에서 다운받을 수 있습니다.
[Releases · protocolbuffers/protobuf](https://github.com/protocolbuffers/protobuf/releases)

## 프로토콜 포맷 정의하기
.proto 파일을 생성하여 프로토콜 포맷을 정의할 수있습니다. .proto 파일에는 직렬화하고자 하는 대상을 `message`로 정의하고 타입의 이름을 지정해 주면 됩니다.

```java
public class Member {  
  
    private Long id;  
    private String name;  
  
    private String email;  
  
    private int age;  

	// constructor, getter, setter ...
}
```

proto
```proto
syntax = "proto2";  
  
package tutorial;  
  
option java_multiple_files = true;  
option java_package = "com.example.tutorial.protos";  
option java_outer_classname = "AddressBookProtos";  
  
message Person {  
  optional string name = 1;  
  optional int32 id = 2;  
  optional string email = 3;  
  
  enum PhoneType {  
    PHONE_TYPE_UNSPECIFIED = 0;  
    PHONE_TYPE_MOBILE = 1;  
    PHONE_TYPE_HOME = 2;  
    PHONE_TYPE_WORK = 3;  
  }  
  
  message PhoneNumber {  
    optional string number = 1;  
    optional PhoneType type = 2 [default = PHONE_TYPE_HOME];  
  }  
  
  repeated PhoneNumber phones = 4;  
}  
  
message AddressBook {  
  repeated Person people = 1;  
}
```

.proto는 패키지 선언으로 시작하여, 다른 프로젝트 간 이름 충돌을 방지할 수 있다.

패키지 선언 뒤에는 java-specific한 설정을 할 수 있다.
* **java_package** : 생성될 클래스의 패키지를 지정해 줄 수 있다. 만약 명시적으로 지정되지 않는다면, package에서 제공된 값을 따른다.
* **java_outer_classname** : 생성될 Wrapper 클래스의 이름을 결정한다. 만약 명시적으로 제공되지 않는다면, 파일 이름을 Upper Camel Case로 바꾼 값이 기본적으로 제공된다.
* **java_multiple_files** : 생성되는 Wrapper가 여러개가 되는 것을 허용할지에 대한 옵션이다.

### 필드 레이블
필드에는 bool, int32, float, double, string과 같은 자료형을 쓸 수 있다.
 또한 위 예시의 PhoneNumber와 같이 다른 메시지 타입을 사용할 수도 있다. 또한 enum 키워드를 통해 Enum 타입을 지정할 수도 있다.

필드 뒤에 들어오는 수치는 고유한 tag를 지정하는데에 사용되는 marker이다. 1-15는 인코딩하는데에 더 1 바이트가 덜 드므로 반복되는 요소에 사용하는 것이 좋고, optional한 요소에 대해서는 16 이상의 태그를 사용하면 좋다.

* optional : 이 필드는 값이 설정되지 않은 경우 기본 값을 제공하며, 아니라면 시스템 기본 값이 제공된다. (숫자의 경우 0, 문자열의 경우 빈 문자열, boolean의 경우 false) 
* repeated : 0번 이상 반복될 수 잇는 필드들을 의미합니다 <- 이게 의미하는 바가 뭘까요? -> 동적인 크기의 배열에 사용
* required : 필드값이 꼭 제공되어야 하는 경우입니다. 초기화되지 않은 경우 IOException이 발생한다.