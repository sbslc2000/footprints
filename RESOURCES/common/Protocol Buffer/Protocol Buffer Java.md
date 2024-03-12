---
상위 링크: "[[Protocol Buffer]]"
---
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

* **optional** : 이 필드는 값이 설정되지 않은 경우 기본 값을 제공하며, 아니라면 시스템 기본 값이 제공된다. (숫자의 경우 0, 문자열의 경우 빈 문자열, boolean의 경우 false) 
* **repeated** : 0번 이상 반복될 수 잇는 필드들을 의미합니다 <- 이게 의미하는 바가 뭘까요? -> (동적인 크기의 배열에 사용한다고 합니다.)
* **required** : 필드값이 꼭 제공되어야 하는 경우이다. 초기화되지 않은 경우 IOException이 발생한다. 
	* required의 경우 proto3 버전에서는 삭제되었습니다.

## 프로토콜 버퍼 컴파일하기
`protoc ./src/main/java/org/example/AddressBook.proto --java_out=./src/main/java/ `

이때, .proto의 package에 org.example이 적혀있다면 의도된 위치에 파일이 생성된다.

컴파일 이후에 클래스를 온전히 사용하려면 다음 의존성을 추가해주어야 한다.

```groovy
// https://mvnrepository.com/artifact/com.google.protobuf/protobuf-java  
implementation("com.google.protobuf:protobuf-java:3.25.3")
```

## Proto API 살펴보기
컴파일을 수행하면 다음과 같은 클래스들이 생성된다.
![](https://i.imgur.com/sZwKRSn.png)

AddressBook, Person 은 message 지시자를 통해 생성된 클래스이다. Person의 내부 message 지시자를 통해 Person.PhoneNumber도 생성된다.

### 객체 생성

각각의 클래스는 클래스명.newBuilder()를 통해 객체를 생성할 수 있다.
```java
Person.PhoneNumber phoneNumber = Person.PhoneNumber  
        .newBuilder()  
        .setNumber("01042645540")  
        .build();  
  
Person person = Person.newBuilder()  
        .addPhones(phoneNumber)  
        .setEmail("sbslc2000")  
        .setName("Pete")  
        .build();  
  
AddressBook addressBook = AddressBook.newBuilder()  
        .addPeople(person)  
        .build();
```

### 메시지 확인
* **isInitialized()** : 필수 항목이 모두 설정되었는지 확인한다.
* **clear()** : 모든 상태를 빈 값으로 지운다.
* **toString()** : 사람이 읽을 수 있는 메시지 표현을 반환한다.
* **mergeFrom(Message other)** : 빌더의 내용을 other에 병합하여 필드를 덮어씁니다.

### 구문 분석 및 직렬화
* **byte\[] toByteArray()** : 메시지를 직렬화하고 원시 바이트가 포함된 바이트 배열을 반환한다.
* **static Person parseFrom(byte\[] data)** : 주어진 바이트 배열에서 메시지를 구문 분석한다.
* **void writeTo(OutputStream output)** : 메시지를 직렬화하여 outputStream에 전송한다.
* **static Person parseFrom(InputStream input)** : InputStream에서 메시지를 읽고 구문 분석한다.


### 테스트
```java
@Test  
void testProto() {  
    // Given  
    Person.PhoneNumber phoneNumber = Person.PhoneNumber  
            .newBuilder()  
            .setNumber("01042645540")  
            .build();  
  
    Person person = Person.newBuilder()  
            .addPhones(phoneNumber)  
            .setEmail("sbslc2000")  
            .setName("Pete")  
            .build();  
  
    AddressBook addressBook = AddressBook.newBuilder()  
            .addPeople(person)  
            .build();  
  
    byte[] protoBufByteArray = addressBook.toByteArray(); // protobuf 직렬화  
    byte[] javaByteArray = null; // java 직렬화  
  
    try(ByteArrayOutputStream baos = new ByteArrayOutputStream()) {  
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {  
            oos.writeObject(addressBook);  
            javaByteArray = baos.toByteArray();  
        }  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    }  
  
    System.out.println(protoBufByteArray.length); // 34  
    System.out.println(javaByteArray.length); // 451  
}
```

protoBuf로 직렬화한 바이트 크기와 자바의 기본 직렬화를 수행한 바이트 크기가 확연히 차이가 나는 것을 확인할 수 있다.