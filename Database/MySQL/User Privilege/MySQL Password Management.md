---
상위 개념: "[[MySQL User Privilege]]"
---
# MySQL Password Management

## 고수준 비밀번호

MySQL에서 비밀번호의 유효성 체크 규칙을 적용하려면 validate_password 컴포넌트를 이용하면 되는데, 이를 위해서는 다음과 같이 컴포넌트를 설치해야한다.
```
INSTALL COMPONENT 'file://component_validate_password';
```

비밀번호 정책은 크게 다음 3가지 중에서 선택할 수 있으며, 기본값은 MEDIUM으로 자동 설정된다.
* LOW: 비밀번호의 길이만 검증
* MEDIUM: 비밀번호의 길이를 검증하며, 숫자와 대소문자, 그리고 특수문자의 배합을 검증
* STRONG: MEDIUM 레벨의 검증을 모두 수행하며, 금칙어가 포함됐는지 여부까지 검증

금칙어는 validate_password.dictionary_file 시스템 변수에 설정된 사전 파일에 명시된 단어를 포함하고 있는지 검증한다. 특정 패턴을 감지하여 유효성 검증을 하고 싶은 경우 금칙어들을 한 줄에 하나씩 기록해서 저장한 텍스트 파일을 등록하면 된다.

https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10k-most-common.txt

```sql
SET GLOBAL validate_password.dictionary_file='prohibitive_word.data';
SET GLOBAL validate_password.policy='STRONG';
```

