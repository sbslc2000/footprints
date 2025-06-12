---
상위 개념: "[[Log-based Recovery]]"
---
# Force Policy vs No-force Policy
트랜잭션 커밋 시에 수정한 모든 블록의 출력을 강제하는 정책을 Force 정책이라고 한다. 반면 no-force 정책은 트랜잭션이 수정한 블록을 모두 디스크에 출력하지 않아도 트랜잭션이 커밋하는 것을 허용한다.

no-force 정책은 트랜잭션이 더 빨리 커밋할 수 있도록 해주며, 한 블록에 변경사항이 축적되는 것을 허용한다. 이에 따라 많은 시스템에서 표준으로 채택하는 기법은 no-forcedlek.

Log-based Recovery Algorithm은 두 정책 중 어떠한 것을 사용하더라도 올바로 작동한다.