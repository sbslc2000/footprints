* **CONCAT** : 두 문자를 더함
	* `select concat(m.username,'님') from Member m`
* **SUBSTRING** : 문자의 특정 위치를 잘라냄
	* `select substring(m.username, 2, 3)`
* **TRIM** : 좌우 공백을 없앰
* **LOWER**, **UPPER** : 대소문자 변환
* **LENGTH** : 문자 길이 반환
* **LOCATE** : 첫번재로 매칭되는 문자열의 인덱스 반환
* **ABS**, **SQRT**, **MOD** : 절대값, 제곱근, 모듈러 연산
* **SIZE**, **INDEX** : JPA 용도
	* SIZE: 매핑된 컬렉션의 크기를 반환
		* `select size(t.members) from Team t`
	* INDEX: @OrderColumn을 사용할 때 사용하지만, 사용 권장 x

이러한 요소들인 JPQL이 제공하는 표준 함수로 데이터베이스에 상관없이 사용할 수 있다.
만약 이러한 것으로 해결할 수 없는, 혹은 DB에 특화된 문법이 있는 경우 사용자 정의 함수를 호출 할 수 있다. 이를 위해서는 사용하는 DB 방언을 상속받고 사용자 정의 함수를 등록하는 과정이 필요하다. (Hibernate에서는 MySQL8Dialect 클래스에서 registerFunction이라는 메서드가 호출됨)

![](https://i.imgur.com/Chij6jZ.png)

```
select function('group-concat',i.name) from Item i;
```