---
상위 링크: "[[Dangling Pointer]]"
---
# Tombstone Approach
![](https://i.imgur.com/hbuJQLq.png)

Dangling Pointer의 문제를 해결하는 방법으로 **Tombstone**이라고 부르는 특수한 공간을 통해 동적 변수들을 관리하는 방법이다.

Tombstone Approach를 사용하는 시스템에서 모든 포인터는 사용하는 동적변수의 공간을 직접 가리키지 않고, Tombstone의 주소를 가리켜 Tombstone이 저장하고 있는 실제 사용하고 있는 주소로 이중 연결을 통해 접근한다.

이러한 구조는 하나의 포인터를 통해 동적 할당된 공간이 해제되면 Tombstone은 해당 주소를 NIL로 바꾸며, 이에 따라 해당 동적 변수를 가리키고 있는 모든 포인터 변수들은 자연스럽게 NIL을 가리키게 되어 오동작을 막는다.

이 구조라면 Danling Pointer의 문제는 해결하지만, tombstone에 필요한 공간적 비용과 참조 할 때마다 이중 접근해야하는 시간적 비용이 발생한다.


