---
상위 링크: "[[Domain-Driven Development]]"
---
# Anemic Domain Model
Anemic Domain Model 방식에서 엔티티는 일반적으로 상태를 표현하는 필드와, 이 값을 읽고 조작하기 위한 getter, setter만 포함하고 어떤 도메인 로직도 갖고 있지 않다.

유스케이스는 도메인 로직을 담고 있어, 비즈니스 규칙을 검증하고, 엔티티의 상태를 바꾼다.