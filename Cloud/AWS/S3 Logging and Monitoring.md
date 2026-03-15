---
상위 링크: "[[AWS S3]]"
---
# S3 Logging and Monitoring
S3의 모니터링 도구는 "무슨 일이 있었는가"를 기록하는 쪽과 "이상한 일이 벌어지고 있는가"를 탐지하는 쪽으로 나뉜다.

## 기록 (Logging)

### CloudTrail
CloudTrail은 S3에서 발생하는 API 호출을 JSON으로 기록한다. 누가, 언제, 어떤 IP에서, 어떤 작업을 했는지 추적할 수 있다. 교차 계정 전달, 로그 암호화, 무결성 검증을 모두 지원하며, AWS가 공식적으로 권장하는 로깅 방식이다.

CloudTrail은 S3 이벤트를 관리 이벤트와 데이터 이벤트로 구분한다. 관리 이벤트는 버킷 수준 작업(CreateBucket, PutBucketPolicy, PutBucketEncryption 등)으로, 자동으로 기록되며 추가 비용이 없다. 데이터 이벤트는 객체 수준 작업(GetObject, PutObject, DeleteObject 등)으로, 기본적으로 비활성화되어 있으며 별도로 켜야 하고 추가 요금이 발생한다. 

하나의 이벤트가 발생하면 eventTime(언제), eventName(어떤 작업), sourceIPAddress(요청자 IP), userIdentity(누가), userAgent(어떤 도구로), requestParameters(요청 상세), resources(대상 리소스 ARN) 등의 필드가 기록된다. 모든 객체 작업을 기록하면 비용이 급증할 수 있으므로, 고급 이벤트 선택기를 통해 특정 API만, 읽기 또는 쓰기만, 특정 버킷만 필터링하여 기록할 수 있다.

전달 속도는 5~15분이다. [[AWS KMS]]의 SSE-KMS를 사용하는 경우 Decrypt, GenerateDataKey 같은 데이터 이벤트도 CloudTrail을 통해 감사할 수 있으며, 이는 SSE-S3에서는 불가능한 기능이다.

### S3 서버 액세스 로그
서버 액세스 로그는 버킷에 대한 모든 요청을 텍스트 파일로 기록하는 기능이다. CloudTrail보다 원시적이지만, 로깅 자체에 추가 비용이 발생하지 않고 저장 비용만 청구된다.

다만 제한사항이 많다. 전달 속도가 수 시간으로 실시간 모니터링에 적합하지 않고, 로그 암호화, 무결성 검증, 교차 계정 전달, 검색 UI 모두 지원하지 않는다. CloudWatch Logs 같은 다른 시스템으로 직접 전달하는 것도 불가능하다. 다만 인증 실패 요청이나 수명 주기 작업 등 CloudTrail이 기록하지 않는 일부 요청을 기록할 수 있다는 점에서 보완적으로 사용된다.

## 탐지 (Detection)

### GuardDuty S3 Protection
GuardDuty는 CloudTrail 로그를 기계학습으로 자동 분석하여 비정상적인 행동을 탐지하는 서비스이다. 평소와 다른 IP에서의 대량 다운로드나 비정상적 API 호출 패턴 같은 위협을 자동으로 식별한다. 활성화만 하면 되고 별도의 규칙을 설정할 필요가 없다.

### CloudWatch
CloudWatch는 S3의 메트릭(요청 수, 지연 시간, 오류율 등)을 수집하고, 임계값을 넘으면 SNS나 Auto Scaling 정책과 연동하여 알람을 발송한다. "무슨 일이 있었나"보다는 "지금 상태가 정상인가"에 초점이 있다.