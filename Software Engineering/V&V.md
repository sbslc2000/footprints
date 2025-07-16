---
상위 링크: "[[Software Engineering]]"
---
# Verification and Validation

![](https://i.imgur.com/fyBxfAs.png)

소프트웨어 V&V (Verification & Validation) 은 소프트웨어 생명 주기 전반에 걸쳐 다음 내용들을 보장하기 위해 수행되는 과정이다.

* 검증(Verification)
	* 개발 프로세스의 각 단계가 올바르게 수행되었는지 확인
	* 제품을 올바르게 만들고 있는가? *Do things right?*
	* 주로 Inspections과 Reviews로 수행한다.
* 확인(Validation)
	* 각 단계에서 생성된 산출물이 이전 단계에서 정의된 요구사항을 충족하는지 확인
	* 올바른 제품을 만들고 있는가? *Do right things?*
	* 주로 테스팅을 통해 수행한다.

## Inspections
Inspections란 소프트웨어 개발 과정에서 정적 산출물 (Ex. 코드, 설계 문서, 요구사항 명세서)을 체계적으로 검토하여 결함을 발견하고 수정하는데에 사용되는 소프트웨어 품질 보증 기법이다. 이는 주로 오류를 초기에 발견하고, 수정 비용을 줄이기 위해 개발 프로세스 중 비교적 초기 단계에서 수행한다.

* Analysis of the **static** system representation
* 시스템의 실제 실행을 필요로 하지는 않는다.

## Testing
[Testing](../Test/Testing.md)이란 소프트웨어가 의도대로 동작하는지를 보여주는 도구이며, 소프트웨어의 결함을 찾는 것을 도와준다. 물론 테스팅은 결함이 있다는 것을 밝히는 도구에 불과하고, 결함이 없음을 보장해주지는 않는다.