```java
@Entity
public class Member {

}
```
# 테이블 매핑 
@Entity가 붙은 클래스는 JPA가 관리하여 테이블과 객체를 매핑한다.
@Entity가 붙은 클래스는..
* 리플렉션과 같은 동적 기술을 사용하여 내부 로직이 수행되기 때문에 **기본 생성자**가 필수이다. 
* final 클래스, enum, interface, inner 클래스에는 사용할 수 없다.

매핑 시 테이블 정보를 제공할 때에는 @Table 애노테이션을 사용할 수 있다.
```java
@Entity
@Table(name = "MBR")
public class Member {
	...
}
```

Kotlin의 경우 기본 생성자를 만드는 과정이 까다롭다. 이를 해결하기 위해 Kotlin에서는 no-arg 플러그인을 제공한다.


# 필드와 컬럼 매핑
```java
@Entity
public class Member {
	@Id
	private Logn id;

	@Column(name="name")
	private String username;

	private Integer age;

	@Enumerated(EnumType.STRING)
	private RoleType roleType;

	@Temporal(TemporalType.TIMESTAMP)
	private Date createDate;

	@Temporal(TemporalType.TIMESTAMP)
	private Date lastModifiedDate;

	@Lob
	private String description;

	@Transient
	private int temp;
	...
}
```

| 애노테이션  | 설명            |
| ----------- | --------------- |
| @Column     | 컬럼 매핑       |
| @Temporal   | 날짜 타입 매핑  |
| @Enumerated | enum 타입 매핑  |
| @Lob        | BLOB, CLOB 매핑 |
| @Transient  | 특정 필드를 컬럼에서 무시                |

## @Column
| 속성                  | 설명                                                                                       | 기본값                              |
| --------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------- |
| name                  | 필드와 매핑할 테이블의 컬럼 이름                                                           | 객체의 필드 이름                    |
| insertable, updatable | 등록, 변경 가능 여부                                                                       | TRUE                                |
| nullable(DDL)         | null 값의 허용 여부를 설정한다. false로 설정하면 DDL 생성 시에 not null 제약조건이 붙는다. |                                     |
| unique(DDL)           | @Table의 uniqueConstraints와 같지만 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용한다.     |                                     |
| columnDefinition(DDL) | 데이터베이스 컬럼 정보를 직접 줄 수 있다. (ex. varchar(100) default 'EMPTY')               | 필드의 자바 타입과 방언 정보를 사용 |
| length(DDL)           | 문자 길이 제약조건, String 타입에만 사용한다.                                              | 255                                 |

## @Enumerated
Enum 타입을 매핑할 때 사용한다.
**ORDINAL은 사용하지 않는게 좋다!** <- Enum의 순서 변경에 대해 값이 꼬이게 될 수 있음
* EnumType.ORDINAL : enum 순서를 데이터베이스에 저장, 기본 값
* EnumType.STRING : enum 이름을 데이터베이스에 저장
## @Temporal
날짜 타입 (java.util.Date, java.util.Calendar)을 매핑할 때 사용한다.
LocalDate 타입은 date 타입으로, LocalDateTime은 timestamp로 매핑이 되어 별도로 사용하지 않아도 된다.
## @LOB
매핑하는 타입에 따라 DB Column의 type이 바뀐다.
* CLOB : String, char[], java.sql.CLOB
* BLOB: byte[], java.sql.BLOB
## @Transient
필드를 매핑의 대상에서 제외하고자 할 때, 메모리상에서만 임시로 값을 보관해야할 때 사용한다.