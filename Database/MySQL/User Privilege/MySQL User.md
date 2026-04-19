---
상위 개념: "[[MySQL]]"
---
# MySQL User 

## 사용자 식별

MySQL에서 사용자는 사용자의 계정 뿐만 아니라 사용자의 접속 지점(클라이언트가 실행된 호스트명이나 도메인, IP 주소)도 계정의 일부가 된다. 따라서 MySQL에서 계정을 언급할 때는 항상 아이디와 호스트를 함께 명시해야한다.

```
'svc_id'@'127.0.0.1'
```
위 계정 정보는 'svc_id'라는 아이디로 서버가 가동중인 로컬 호스트에서 접속할 때만 사용될 수 있다. 다른 PC에서는 'svc_id'로 로그인할 수 없다. 만약 모든 외부 컴퓨터에서 접속이 가능한 사용자 계정을 생성하고 싶다면 사용자 계정의 호스트 부분을 와일드카드 문자 \%로 대체하면 된다.

동일한 사용자가 있을 때 MySQL 서버는 범위가 가장 작은 것을 먼저 선택한다. 다음과 같은 계정 정보가 있다고 가정하자.

```
'svc_id'@'192.168.0.10' (비밀번호: 123)
'svc_id'@'%' (비밀번호: abc)
```

이 경우 ip가 192.168.0.10인 호스트에서 svc_id로 비밀번호를 abc로 입력하면, 로그인에 실패한다. 두 계정 가운데 범위가 더 좁은 것은 첫번째 계정이기 때문이다.

## 시스템 계정과 일반 계정
MySQL 8.0부터 계정은 SYSTEM_USER 권한을 가지고 있느냐에 따라 시스템 계정(System Account)와 일반 계정(Regular Account)로 나뉜다.

시스템 계정은 시스템 계정과 일반 계정을 관리(생성 삭제 및 변경)할 수 있지만, 일반 계정은 시스템 계정을 관리할 수 없다. 또한 다음과 같이 데이터베이스 서버 관리와 관련된 중요 작업은 시스템 계정으로만 수행할 수 있다.

* 계정 관리(계정 생성 및 삭제, 그리고 계정의 권한 부여 및 제거)
* 다른 세션(Connection) 또는 그 세션에서 실행 중인 쿼리를 강제 종료
* 스토어드 프로그램 생성 시 DEFINER를 타 사용자로 설정

MySQL 서버에는 다음과 같이 내장된 계정들이 있는데, 'root'@'localhost'를 제외한 3개의 계정은 내부적으로 각기 다른 목적으로 사용되므로 삭제되지 않도록 주의해야한다.

* 'mysql.sys'@'localhost': MySQL 8.0부터 기본으로 내장된 sys 스키마의 객체(뷰나 함수, 그리고 프로시저)들의 DEFINER로 사용되는 계정
* 'mysql.session'@'localhost': MySQL 플러그인이 서버로 접근할 때 사용되는 계정
* 'mysql.infoschema'@'localhost': information_schema에 정의된 뷰의 DEFINER로 사용되는 계정

위 3개의 계정은 처음부터 잠겨 있는 상태이므로, 의도적으로 잠긴 계정을 풀지 않는 한 악의적인 용도로 사용할 수 없으므로 보안을 걱정하지는 않아도 된다.

> [!info] Definer란?
> MySQL에서 뷰/함수/프로시저를 만들 때 "이 객체는 어느 계정의 권한으로 실행할 것인가"를 지정하는 속성입니다. DEFINER 계정이 존재하지 않으면 해당 뷰를 조회할 때 ERROR 1449: The user specified as a definer does not exist 에러가 발생합니다.                                                                        

> [!info] sys 스키마
> performance_schema의 저수준 성능 지표를 DBA가 읽기 쉬운 형태로 가공해 보여주는 뷰/프로시저 모음입니다. 예: sys.host_summary, sys.schema_table_statistics,                                   
  sys.statements_with_full_table_scans 등.

## 계정 생성
MySQL 5.7 버전까지는 GRANT 명령으로 권한의 부여와 동시에 계정 생성이 가능했지만, 8.0버전 부터는 계정의 생성은 CREATE USER 명령으로, 권한 부여는 GRANT 명령으로 구분해서 실행하도록 바뀌었다. 

계정을 생성할 때에는 다음과 같은 다양한 옵션을 설정할 수 있다.
* 계정의 인증 방식과 비밀번호
* 비밀번호 관련 옵션 (비밀번호 유효 기간, 비밀번호 이력 개수, 비밀번호 재사용 불가 기간)
* 기본 역할
* SSL 옵션
* 계정 잠금 여부 

```sql
CREATE USER 'user'@'%'
	IDENTIFIED WITH 'mysql_native_password' BY 'password'
	REQUIRE NONE
	PASSWORD EXPIRE INTERVAL 30 DAY
	ACCOUNT UNLOCK
	PASSWORD HISTORY DEFAULT
	PASSWORD REUSE DEFAULT
	PASSWORD REQUIRE CURRENT DEFAULT;
```

### IDENTIFIED WITH

```sql
create user 'user'@'%' identified with 'mysql_native_password' by 'password'
```
identified with는 사용자의 인증 방식(인증 플러그인의 이름)과 비밀번호를 설정한다. MySQL 서버의 기본 인증 방식을 사용하고자 한다면 `IDENTIFIED BY 'password'` 형식으로 명시해야한다.

MySQL 서버에서는 다양한 인증 방식 (Native Pluggable Authentication, Caching SHA-2 Pluggable Authentication, LDAP Pluggable Authentication)을 플러그인 형태로 제공한다. MySQL 5.7 까지는 Native Authentication이 기본 인증 방식이었으나, 8.0부터는 Caching SHA-2 Authentication이 기본 인증으로 바뀌었다.

Caching SHA-2 Authentication은 서버 메모리에 사용자의 패스워드 해시값이 캐시되어 있는지에 따라 두 가지 경로로 인증을 수행한다. 캐시에 해시값이 있다면 Challenge-Response 방식의 Fast Authentication으로, 없다면 RSA 키 쌍 또는 SSL/TLS 터널을 통해 패스워드를 안전하게 전송하는 Full Authentication으로 처리된다.

> [!info] Challenge-Response 인증
> 패스워드 자체를 네트워크로 전송하지 않고도 클라이언트가 올바른 패스워드를 알고 있는지 검증하는 기법이다. Fast Authentication은 다음 순서로 진행된다.
> 1. 클라이언트가 로그인을 요청하면, 서버는 매 요청마다 새로 생성한 무작위 난수(Nonce)를 Challenge로 보낸다.
> 2. 클라이언트는 사용자가 입력한 패스워드와 서버가 보낸 난수를 조합해 SHA-256 해시와 XOR 연산으로 섞은 결괏값을 Response로 되돌려준다.
> 3. 서버는 캐시에 보관 중인 패스워드 해시값과 자신이 보낸 난수로 동일한 연산을 수행한 뒤, 클라이언트가 보낸 Response와 일치하는지 비교한다.
>
> 해시 함수의 단방향성 덕분에 중간자가 난수와 Response를 모두 가로채더라도 원본 패스워드를 역산할 수 없으며, 난수가 매 요청마다 새로 발급되므로 이전 Response를 재전송해 인증을 통과하는 재전송 공격(Replay Attack)도 차단된다. 그 결과 SSL/TLS 터널 없이도 가벼운 해시 연산만으로 인증이 성립한다.


### REQUIRE
MySQL 서버에 접속할 때 암호화된 SSL/TLS 채널을 사용할 지 여부를 설정한다.

### PASSWORD EXPIRE
비밀번호의 유효기간을 설정하는 옵션이며, 별도로 명시하지 않으면 default_password_lifetime 시스템 변수에 저장된 기간으로 유효 기간이 설정된다.

### PASSWORD HISTORY
한 번 사용했던 비밀번호를 재사용하지 못하게 설정하는 옵션이다. 이 데이터는 mysql DB의 password_history 테이블을 사용한다. `PASSWORD HISTORY n` 을 통해 비밀번호 이력을 최근 n개까지 저장한 뒤, 이력에 있는 비밀번호는 재사용 금지시킬 수 있다.

### PASSWORD REUSE INTERVAL
한 번 사용했던 비밀번호의 재사용 금지 기간을 설정하는 옵션이다.

### PASSWORD REQUIRE
비밀번호가 만료되어 새로운 비밀번호로 변경할 때 현재 비밀번호(변경하기 전 만료된 비밀번호)를 필요로 할지 말지를 결정하는 옵션이다. 

### ACCOUNT LOCK / UNLOCK
계정 생성 시, 또는 ALTER USER 명령을 통해 계정 정보를 변경할 때 계정을 사용하지 못하게 잠글지 여부를 결정한다.


> [!info] 계정 잠금 (account_lock)
> account_locked = 'V'로 설정되어 있으면, 외부에서 이 계정으로 로그인 시도 시 거부된다. 다음 쿼리로 확인해볼 수 있다.
> ```
> SELECT User, Host, account_locked FROM mysql.user
  WHERE User LIKE 'mysql.%';
> ```
