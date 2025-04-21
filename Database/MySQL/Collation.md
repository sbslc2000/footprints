---
상위 링크: "[[MySQL]]"
---
# 콜레이션(Collation)

## 콜레이션이란?
* 문자를 비교하거나 정렬할 때 사용되는 규칙
	* ex. 알파벳 a와 알파벳 A는 같은가? 둘 중 정렬에서는 무엇이 우선하는가?
* 문자집합 (Character Set)에 종속적 -> 코드값을 기반으로 동작하기 때문
	* 문자와 코드 값(코드 포인트)의 조합이 정의돼있는 것이 문자집합
		* e.g "A=U+0041"
* MySQL에서 모든 문자열 타입 컬럼은 독립적인 문자집합과 콜레이션을 가질 수 있음
* 사용자가 특별히 지정하지 않는 경우, 서버에 설정된 문자집합의 디폴트 콜레이션으로 자동 설정됨

## 콜레이션 종류

### 콜레이션 목록 확인
* `SHOW COLLATION` 명령으로 사용가능한 콜레이션 목록 확인 가능
![](https://i.imgur.com/OeZfab3.png)
	* 이 곳에서 콜레이션의 문자집합, 기본속성인지, 패딩처리를 하는지에 관련된 내용을 볼 수 있다.

### MySQL에서 콜레이션 네이밍 컨벤션

```문자집합_언어종속_UCA버전_민감도```

* 문자집합 
	* utf8mb4
	* utf8mb3
	* ...
* 언어 종속
	* 선택적인 속성으로, 생략될 수 있음
	* 특정 언어에 대해 해당 언어에서 정의한 정렬 순서에 의해 정렬 및 비교를 수행 (다른 언어들에는 적용되지 않음)
	* locale code ("kr") 또는 language ("korean")으로 표시됨
* UCA = Unicode Collation Algorithm Version
	* 선택적 필드임
	* 유니코드 기반의 문자집합 콜레이션에서 사용하는 문자열 비교 표준 알고리즘
* 민감도
	* Accent-insensitive인지 - \_ai,  \_as
	* Case-insensitive인지 \_ci \_cs
	* Binary -> 문자열을 binary로 비교하는지


## 콜레이션 동작 방식
### 유니코드 기준
* 데이터 저장
	* 코드 포인트 값을 인코딩해서 저장
	* (예시) 일반적으로 많이 사용되는 UTF-8 인코딩 방식
![](https://i.imgur.com/gHZjkDy.png)

* 데이터 비교
	* DUCET(Default Unicode Collation Element Table)에 정의된 가중치 값을 바탕으로 비교
	* 가중치 값은 \[Primary.Secondary.Tertiary] 와 같이 단계적으로 구성됨
	* ![](https://i.imgur.com/omwJ1V6.png)
	* 콜레이션에 따라 사용되는 가중치 값이 달라짐
		* ai_ci는 1단계 가중치 값 (Primary Weight) 까지,
		* as_ci는 2단계 가중치 값 (Secondary Weight) 까지,
		* as_cs는 3단계 가중치값 (Tertiary Weight) 까지 사용
	* 한글 음절의 경우 분해해서 가중치 값을 조합한 후 비교
	* WEIGHT_STRING() 함수를 통해 문자열의 가중치 값 확인 가능
	* 문자 'ㄱ'에 대한 가중치 값
	* ![](https://i.imgur.com/BY0ciM0.png)
	* 문자 'ㄱ/ㄴ/ㄷ'에 대한 정렬 및 가중치 값 확인
	* ![](https://i.imgur.com/QEjuyzb.png)

## 콜레이션 설정
* 기본적으로 MySQL 서버에 설정된 문자집합(Character Set)의 디폴트 콜레이션으로 글로벌하게 설정됨
	* 문자집합과 콜레이션 모두 특별히 값을 설정하지 않은 경우, 기본적으로 서버는 utf8mb4 문자집합 & utf8mb4_0900_ai_ci 콜레이션으로 설정됨
	* ![](https://i.imgur.com/urkVsNh.png)
	* character_set_server&collation_server 설정 변수를 통해 원하는 문자집합 및 콜레이션 설정 가능
* 데이터베이스/테이블/컬럼 단위로 독립적으로 지정 가능
	* ![](https://i.imgur.com/otlGHvY.png)

## 콜레이션 사용 시 주의사항
* 서로 다른 콜레이션을 가진 컬럼들 값 비교 시 에러 발생
![](https://i.imgur.com/DH07tY8.png)

* 쿼리 WHERE절에서 콜레이션 변경 시, 일반 인덱스가 있더라도 사용 불가하다.
![](https://i.imgur.com/NDgJJXK.png)

![](https://i.imgur.com/DPFQvVK.png)
인덱스 데이터는 인덱스되는 컬럼의 콜레이션 규칙에 따라 정렬되어 저장된다.

![](https://i.imgur.com/fFCq77q.png)
이런 방식으로 처리하면 된다! (함수 기반 인덱스)

* 고유키도 콜레이션의 영향을 받는다.
![](https://i.imgur.com/E6nnyXN.png)

* 기본 콜레이션(utf8mb4_0900_ai_ci)에서의 한글 비교 문제 
	* "가"와 "ㄱㅏ" 가 동일하게 인식 됨
	* ![](https://i.imgur.com/LuneqT8.png)
	* utf8mb4_0900_ai_ci에서의 '가' & 'ㄱㅏ' 문자열 비교 ![](https://i.imgur.com/ivEpXkW.png)
		* 한글에 대해서 초성, 중성, 종성 별로, 또한 개별 문자별로 가중치가 테이블에 존재한다.
		* 한글의 경우 음절 문자는 분해를 통해 가중치를 계산한다.
		* "가" 의 경우 초성 ㄱ의 가중치와 중성ㅏ의 첫번째 가중치를 연결한 값을 갖는다.
		* "ㄱㅏ"의 경우, ㄱ의 가중치와 ㅏ의 가중치를 연결한 값을 갖는다. 
			* 이들은 서로 다른 테이블에 있지만, Primary Weight는 동일하기 때문에 같은 값을 갖는다.
		* 따라서 "각" 과 "가ㄱ"은 다르다. (각의 종성 ㄱ은 다른 Primary Weight를 갖기 때문에)
	* utf8mb4_0900_ai_ci에서의 '각'과 'ㄱㅏㄱ' 문자열 비교 ![](https://i.imgur.com/Eza7Rx7.png)
		* 종성 ㄱ은 다른 Primary Weight를 가지기 때문에 두 문자열은 서로 다른 값으로 인식됨
	* utf8mb4_0900_as_cs에서의 '가' & 'ㄱㅏ' 문자열 비교 ![](https://i.imgur.com/hAMde7V.png)
		* 이때는 Secondary, Tertiary 모두 사용한다.
		* 초성 ㄱ와 문자 ㄱ의 세번째 weight이 다르기 때문에 서로 다른 것으로 구분
	* 이 처럼 기본 콜레이션에서의 한글 비교 문제가 있다.
		* 정확한 한글 구분이 필요한 경우 다른 콜레이션을 사용하거나, 쿼리 where 절에서 다음과 같이 비교조건을 추가해서 사용 가능
		* 이 경우 'ㄱㅏ을'인 데이터는 제외됨 ![](https://i.imgur.com/b3KGMNM.png)
			* unicode_520_ci에는 '가'와 'ㄱㅏ'를 다른 것으로 구분

![](https://i.imgur.com/b2bfmQk.png)

![](https://i.imgur.com/GotivJs.png)

* 대소문자 구분을 위한 콜레이션
	* 기본 콜레이션은 ci 로, 대소문자를 구분하지 ㅇ낳음
	* 동일한 utf8mb4 문자집합에 속한 콜레이션 중 대소문자를 구분하는 콜레이션은
		* **utf8mb4_bin**
			* 후행 공백 인식을 원하지 않는 경우 사용하자. ex. 'a '와 'a' 가 동일하게 여겨졌으면
		* **utf8mb4_0900_bin**
			* 후행 공백 인식을 원하거나 상관이 없을 때 모든 문자를 명확히 구별하고 싶다면
		* **utf8mb4_0900_as_cs**
			* 후행 공백 인식을 원하거나 상관이 없을 때 대소문자만 잘 구분되면 충분하다면
	* 비교표 ![](https://i.imgur.com/MIQ2Q6w.png)
