---
상위 개념: "[[Non-ordered Index]]"
---
# Hash Index
해시 인덱스는 메인 메모리에서 인덱스를 구축할 때 널리 사용되는 기술이다. 조인 작업을 수행하기 위해 임시로 생성될 수도 있고, 메인 메모리 데이터베이스에서 영구적인 구조로 사용될 수도 있다.

* 버켓(bucket) : 한 개 또는 많은 수의 레코드를 저장할 수 있는 저장 공간의 단위

일반적으로 [해시 테이블](Hash%20Table)은 해시 충돌 상황에서 개방 주소법과 체이닝(폐쇄 주소법) 방식을 사용할 수 있지만, 데이터베이스 인덱스에서는 체이닝을 선호한다. 삭제를 효율적으로 처리할 수 없어 적합하지 않기 때문이다. 

## 정적 해싱과 동적 해싱
인덱스를 생성할 때 버켓 개수가 고정되어 있는 해시 인덱싱을 정적 해싱(static hashing)이라고 한다. 정적 해싱은 레코드가 계속 추가되는 경우 검색에 여러 오버플로 버켓을 탐색해야 할 수 있으므로 비효율적이다. 데이터가 계속 추가되는 것이 만연한 데이터베이스 시스템에서 이는 치명적이다. 이를 해결하는 방법은 다음과 같다.

1. **추가될 레코드의 개수를 감안하여 버킷 개수를 크게 잡는다.**
하지만 이는 리소스를 비효율적으로 사용하게 된다.
2. **대부분의 버켓이 가득 찬다면, 버캣 개수를 늘려 다시 해시 인덱스를 작성한다.**
하지만 이러한 재구축은 릴레이션의 크기가 큰 경우 시간이 많이 소요되어 정상적인 시스템 운영을 방해할 수 있다.
3. **동적 해싱(dynamic hashing) 사용**
버켓 개수를 레코드 수에 따라 점진적으로 늘릴 수 있는 기법을 사용한다. 주로 사용되는 기법은 선형 해싱(linear hashin)과 확장성 해싱(extendable hashing)이다.

## 단점
1. 범위 질의를 지원하지 않는다. 연속된 키 값을 가지는 레코드 포인터들도 해시 함수에 의한 임의의 공간에 할당되기 때문이다.
2. 여러 개의 레코드가 동일한 키 값을 가지거나, 해시 함수가 임의성을 보장하지 못하는 경우 특정 버켓에 엔트리들이 몰리는 치우침(skew)현상이 발생할 수 있다.