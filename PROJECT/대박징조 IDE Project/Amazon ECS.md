- 간단한 API 호출을 통해 컨테이너 기반 애플리케이션을 시작하고 중지할 수 있다. : **컨테이너 오케스트레이션**
- EKS: Elastic Kubernetes Service로, 비슷한 역할을 수행하지만 학습 곡선으로 인해 원활한 사용 어려울 것 같아 ECS 선택
- 클러스터링과 Auto Scaling등을 효과적으로 지원한다.
- Amazon ECR : 컨테이너 이미지 저장소
- Task Definition :
    - EC2, Fargate, External 시작 유형 호환성 선택
    - 사용할 컨테이너 이미지 설정
    - 애플리케이션을 위해 개방할 포트 설정
    - CPU/메모리 리소스 할당 설정
    - 데이터 볼륨 설정
- Service:
    - Load Balancing과 Auto Scaling 등의 설정을 할 수 있다.

Fargate : 컨테이너를 배포하고 관리할 수 있는 서버리스 컴퓨팅 엔진

### EC2 vs Fargate

EC2: 독립된 환경이 있고 운영체제를 갖고 있는 컴퓨팅 리소스

Fargage: EC2의 서버리스 버전. 독립된 운영체제가 없다.

**사용자 IDE는 독립된 운영체제를 가져야 하는가?**

# 웹 소켓
