---
상위 링크: "[[Computer Network]]"
---
# MAC
이제 **공유 이더넷 링크에 대한 접근을 제어하는 알고리즘**에 주목해보겠습니다. 이 알고리즘은 일반적으로 **이더넷의 미디어 접근 제어** *MAC, Media Access Control* 라고 불리며, 주로 네트워크 어댑터 하드웨어에서 구현됩니다. 여기서는 하드웨어 자체를 설명하지 않고, 이 하드웨어가 구현하는 알고리즘에 초점을 맞추겠습니다. 먼저, 이더넷의 **프레임 형식***frame format*과 **주소***address*에 대해 설명하겠습니다.

### Frame Format
이더넷 프레임은 그림 4에 제시된 형식에 따라 정의됩니다. 64비트 프리엠블(preamble)은 수신기가 신호와 동기화할 수 있도록 해주며, 0과 1이 번갈아 나오는 연속된 비트 시퀀스로 구성됩니다. 송신자와 수신자는 각각 48비트 주소로 식별됩니다. 패킷 유형 필드(packet type field)는 여러 고계층 프로토콜 중 어느 프로토콜로 프레임을 전달해야 하는지를 식별하는 데 사용되는 **디멀티플렉싱 키(demultiplexing key)** 역할을 합니다. 각 프레임에는 최대 1500바이트의 데이터가 포함될 수 있으며, 최소한 46바이트의 데이터를 포함해야 합니다. 이 최소 프레임 크기를 유지하는 이유는 충돌을 감지할 수 있을 만큼 충분히 프레임이 길어야 하기 때문입니다. 이 부분에 대해서는 아래에서 더 자세히 설명하겠습니다. 마지막으로, 각 프레임은 **32비트 CRC(Cyclic Redundancy Check)** 를 포함합니다. 이전 섹션에서 설명한 HDLC 프로토콜과 마찬가지로 이더넷도 **비트 지향 프레이밍(bit-oriented framing)** 프로토콜입니다.

호스트의 관점에서 볼 때, 이더넷 프레임은 **14바이트의 헤더**를 가집니다. 이 헤더에는 두 개의 6바이트 주소와 2바이트의 유형 필드가 포함됩니다. 송신 어댑터는 프리엠블과 CRC를 프레임에 추가한 후 전송하며, 수신 어댑터는 수신할 때 이들을 제거합니다.
류를 감지하고 해당 프레임을 무시할 수 있습니다.

### Address
이더넷에 연결된 각 호스트, 더 나아가 전 세계의 모든 이더넷 호스트는 고유한 이더넷 주소를 가지고 있습니다. 엄밀히 말하면, 이 주소는 호스트가 아닌 **어댑터(adaptor)** 에 할당된 것으로, 보통 ROM에 영구히 저장됩니다. 이더넷 주소는 사람이 읽기 쉽게 표현되며, 여섯 개의 숫자들이 콜론으로 구분되어 나열됩니다. 각 숫자는 6바이트 주소의 1바이트에 해당하며, 1바이트를 두 개의 4비트 **니블(nibble)** 로 나누어 각 니블에 대해 하나의 16진수 숫자로 나타냅니다. 선행 0은 생략됩니다. 예를 들어, 8:0:2b:e4:b1:2는 이더넷 주소의 사람이 읽기 쉬운 표현입니다.

```c
00001000 00000000 00101011 11100100 10110001 00000010
```

모든 어댑터가 고유한 주소를 갖도록 하기 위해, 이더넷 장치 제조업체는 각각 **다른 접두사(prefix)** 를 할당받아 그들이 생산하는 모든 어댑터에 이 접두사를 추가해야 합니다. 예를 들어, Advanced Micro Devices(AMD)는 **24비트 접두사 080020**(또는 8:0:20)을 할당받았습니다. 각 제조업체는 할당받은 접두사 뒤에 붙는 주소의 **접미사(suffix)** 가 고유하도록 관리하여 전 세계 모든 어댑터가 고유한 이더넷 주소를 가지게 합니다.