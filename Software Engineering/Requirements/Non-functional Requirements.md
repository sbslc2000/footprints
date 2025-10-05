---
상위 개념: "[[Requirement Analysis]]"
---
# Non-functional Requirements
비기능 요구사항(Non-functional Requirements)란 시스템이 '무엇을 해야하는지'가 아니라, '어떻게 동작해야 하는가'를 규정하는 요구사항이다. 시스템의 품질적 특성이나 제약 조건을 다룬다.

> [!example]
> FR: 회원가입 기능이 있어야 한다.
> NFR: 회원가입 기능은 3초 이내에 완료되어야 한다.

## NFR의 구성
비기능 요구사항은 크게 두 가지로 나뉜다.

### Quality Attributes
품질 속성이란 시스템이 가져야할 사용성, 신뢰성, 성능 등 시스템의 품질을 정의하는 요소이다.

* Usability: 사용자가 쉽게 사용할 수 있어야 함
* Reliability: 안정적이고 오류 없이 동작해야함
* Performance: 빠르고 효율적으로 수행되어야 함
* Security, Scalability, Maintability 등도 포함된다.

### Constraints
제약 조건이란 시스템이 개발되거나 운영될 때 따라야하는 기술적 환경적 제약이다.

* ex. 운영체제는 Windows만 사용해야 한다.
* ex. 개발 언어는 Java로 제한된다.

## NFR Classification
비기능 요구사항은 발생 근거에 따라 세 가지로 분류된다.

### Product Requirements
제품 요구사항이란 제품 자체의 품질 특성과 관련된 요구사항이며, 시스템이 특정한 방식으로 동작해야 함을 명시한다.

* 응답 시간은 2초 이내여야 한다.
* 시스템 가용성은 99.9% 이상이어야 한다.

### Organisational Requirements
조직 요구사항이란 개발 및 운영을 수행하는 조직의 정책과 절차로부터 파생되는 규칙들이다.

* 모든 개발은 ISO 9001 표준을 따라야 한다.
* 코드 리뷰 및 테스트 프로세스는 사내 규정을 따라야 한다.

### External Requirements
외부 요구사항은 시스템 외부의 도메인, 법률, 규제 등에서 기인하는 요구사항을 의미한다.

* 개인정보법을 준수해야한다.
* 의료기기법 규정을 만족해야한다.

