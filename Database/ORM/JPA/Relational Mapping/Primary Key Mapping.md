---
상위 링크: "[[../JPA]]"
---
# @Id
해당 필드를 PK로 지정한다.
# @GeneratedValue
Id 값을 자동으로 생성해준다.
* **[IDENTITY](IDENTITY%20전략.md)**:  데이터베이스에 위임
* **[SEQUENCE](SEQUENCE%20전략.md)**: 데이터베이스 시퀀스 오브젝트 사용
	* @SequenceGenerator 필요
* **Table**: 키 생성용 테이블 사용, 모든 DB에서 사용 (Sequence Object를 지원하지 않아도 사용 가능, 하지만 Lock 등이 걸릴 수 있어 성능 저하 가능성이 있음)
	* @TableGenerator 필요
* **AUTO**: Dialect에 따라 자동 지정


