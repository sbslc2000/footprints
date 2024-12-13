## 용어

- **Workflow**
    - 자동화된 전체 프로세스로 하나 이상의 Job 으로 구성되며 Event 에 의해 트리거 되거나 예약될 수 있다.
    - 배포를 포함하여 테스트, infra 구성등 모두 하나로 보는 개념이다.
    - Jenkins 의 Pipeline 과 비슷한 개념이다.
- **Event**
    - Workflow 를 트리거하는 특정한 활동이나 규칙이다.
    - 보통 master, main, release 브랜치에 트리거 규칙을 정해놓곤 한다.
    - 즉, master 브랜치의 코드 변경사항이 있다면 Workflow 를 발동시킨다.
- **Job**
    - 단일 가상 환경에서 실행되는 작업들의 단위이다.
    - 여러 Job과 의존 관계를 맺을 수 있으며 독립, 병렬로 실행될 수 있다.
- **Step**
    - Job 안에서 순차적으로 실행되는 프로세스 단위이다.
- **Action**
    - job 을 구성하기 위한 step 의 조합이다.
    - Github Actions Marcket Place 에 많은 Actions 가 정의되어 있어서 쉽게 가져다 사용할 수 있다.
- **Runner**
    - Github Action Runner 가 설치될 머신으로 Workflow 가 실행될 인스턴스를 의미한다