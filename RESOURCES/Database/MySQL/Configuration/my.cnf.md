# my.cnf

MySQL 서버는 단 하나의 설정 파일을 사용하는데, 리눅스를 포함한 유닉스 계열에서는 my.cnf라는 이름을 사용하고 윈도우 계열에서는 my.ini라는 이름을 사용한다.

MySQL 서버는 시작할 때만 이 설정파일을 참조하는데, 설정파일의 경로는 고정돼있지 않다. 일반적으로 지정된 여러개의 디렉터리를 순차적으로 탐색하면서 처음 발견된 my.cnf를 사용하게 된다.

1. /etc/my.cnf
2. /etc/mysql/my.cnf
3. /usr/etc/my.cnf
4. ~/.my.cnf

MySQL 서버가 어느 디렉터리에서 my.cnf 파일을 읽는지 궁금하다면 `mysqld --verbose --help` 혹은 `mysql --help` 를 실행해보면 된다.

# 구조



#mysql