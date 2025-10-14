---
상위 링크: "[[../Javascript|Javascript]]"
---
# JS Primitive Type
Javascript의 원시 타입에는 number, string, boolean, null, undefined와 ES6에 추가된 Symbol이 있다.

## undefined
undefined는 사용자가 직접 할당하거나, 자바스크립트 엔진이 자동으로 부여해주기도 한다.

자바스크립트 엔진은 사용자가 응당 어떤 값을 지정할 것이라고 예상되는 상황임에도 실제로 그렇게 하지 않았을 때 undefined를 반환한다.

1. 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
2. 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
3. return 문이 없거나 호출되지 않는 함수의 실행 결과

## 기본형도 결국 주솟값을 참조한다.
#todo 