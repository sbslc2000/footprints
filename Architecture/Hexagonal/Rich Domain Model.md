---
상위 링크: "[[Domain-Driven Development]]"
---
# Rich Domain Model
Rich Domain Model은 애플리케이션의 코어에 있는 엔티티에서 가능한 한 많은 도메인 로직이 구현된다. 엔티티들은 상태를 변경하는 메서드를 제공하고, 비즈니스 규칙에 맞는 유효한 변경만을 허락한다.

Rich Domain Model에서 유스케이스는 도메인 모델의 진입점으로 동작한다. 유스케이스는 사용자의 의도만을 표현하면서, 도메인 객체에 실제 작업을 맡긴다.