---
공식 링크: https://protobuf.dev/
---
# Protocol Buffer
> Protocol Buffers are a **language-neutral**, platform-neutral extensible mechanism for serializing structured data.

Protocol Buffer는 언어 중립적, 플랫폼 중립적인 직렬화 매커니즘이다.

## Protocol Buffer로 해결할 수 있는 문제
Protocol Buffer는 구조화된 데이터를 더 적은 패킷을 가진 형태로 직렬화할 수 있게 해준다. 이러한 형태는 **일회성의 네트워크 전송**과 **장기 데이터 저장**에 모두 적합하다. Protocol Buffer는 새로운 데이터가 추가되더라도 기존의 데이터가 무효화되거나 업데이트 되어야할 필요 없이 확장 가능하다.

Protocol Buffer는 구글에서 가장 흔하게 쓰인다. 구글에서는 inter-server communication 뿐만 아니라 disk에 데이터를 아카이빙하는데에도 사용한다.

프로토콜 버퍼에는 message와 service가 있으며, 이는 개발자가 작성한 .proto 파일에 들어있다.

```proto
message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;
}
```

proto compiler는 빌드 타임에 수행되며 다양한 프로그래밍 언어의 파일을 생성할 수 있다. 생성된 파일은 필드에 접근할 수있는 간단한 접근자를 가지고 있으며, 전체 structure를 직렬화하고 parsing할 수있는 메서드를 가지고 있다.

```java
Person john = Person.newBuilder()
    .setId(1234)
    .setName("John Doe")
    .setEmail("jdoe@example.com")
    .build();
output = new FileOutputStream(args[0]);
john.writeTo(output);
```

## Protocol Buffer 사용의 장점
프로토콜 버퍼는 언어 중립적이고 플랫폼 중립적이며 확장 가능한 방식으로 구조화된 데이터를 직렬화하는 모든 상황에 적합하다. 

* Compact한 데이터 저장
* 빠른 파싱
* 다양한 프로그래밍 언어 제공
* 자동 생성 클래스를 통해 최적화된 기능 제공

## Protocol Buffer가 적합하지 않은 경우

1. Protocol Buffer는 전체 메시지를 한 번에 메모리에 로드하는 경향이 있다. 따라서 개별 데이터가 몇 메가바이트를 초과하는 경우 다른 직렬화 방식을 사용하는 것이 좋을 수 있다.
2. 메시지는 압축되지 않으므로, 특수 목적 알고리즘을 사용했을 때에 저장공간을 더 아낄 수 있을 것이다.

