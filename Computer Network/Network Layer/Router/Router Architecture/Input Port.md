---
상위 개념: "[[Router Architecture]]"
---
# Input Port
![](https://i.imgur.com/9UXZ29B.png)
입력 포트(Input port)는 입력 패킷에 대한 물리 계층과 링크 계층 기능을 수행한다. 이후 포워딩 테이블을 참조하여 출력 포트를 결정한다. 포워딩 테이블은 라우팅 프로세서로부터 '입력 라인 카드'에 복사된다.

![](https://i.imgur.com/zgwNW0e.png)

입력 포트에서는 목적지 주소를 검사하여 포워딩 테이블 엔트리에 매치되는 출력 링크 인터페이스로 보내야한다. 이 과정은 최장 프리픽스 매치 규칙(longest prefix matching rule)을 통해 이루어진다.
