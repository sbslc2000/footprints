# UseCase
UseCase란 사용자의 의도를 달성하기 위해 시스템이 수행해야 하는 기능 단위이다. 일반적으로 하나의 유스케이스는 특정한 목적을 가진 절차를 정의하며, 외부 입력을 받아 도메인 로직과 외부 시스템을 적절히 조합하여 결과를 생성하는 역할을 수행한다. 예를 들어 환불 금액 계산, 포인트 적립, 환율 조회 같은 기능이 모두 UseCase에 해당한다.

UseCase는 애플리케이션 계층에 위치하며, 컨트롤러나 핸들러가 요청을 전달하는 대상이 된다. 이 계층은 도메인 객체와 외부 시스템을 조합하는 흐름을 정의하며, 직접 DB나 외부 API를 호출하지 않고 인터페이스(포트)를 통해 접근한다. 이를 통해 관심사를 분리하고 테스트 가능한 구조를 유지할 수 있다.

UseCase는 단일 책임 원칙을 따르며, 명확한 입력과 출력을 가지는 함수로 표현되는 경우가 많다. 외부 환경에 의존하지 않도록 구성하는 것이 이상적이며, 필요 시 정책이나 도메인 서비스를 주입받아 활용한다. 여러 유스케이스를 묶어서 하나의 서비스로 구성하는 경우, 이를 ApplicationService라고 부르기도 하나, DDD나 클린 아키텍처에서는 UseCase 중심의 구조가 더 명확하고 선호되는 방식이다.