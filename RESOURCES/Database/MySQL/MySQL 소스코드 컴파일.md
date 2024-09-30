1. ubuntu 22.04에서 진행합니다.
2. 필요한 라이브러리를 설치합니다.
```
apt-get update
apt-get install -y build-essential cmake git libssl-dev libboost-all-dev bison flex libaio-dev libgcrypt-dev zlib1g-dev libtirpc-dev libncurses5-dev pkg-config
```

3. mysql-server source code를 다운받습니다.
```
# MySQL 소스 코드 저장할 디렉토리 생성
mkdir -p ~/mysql-source
cd ~/mysql-source

# MySQL 소스 코드 클론 (기본 브랜치)
git clone https://github.com/mysql/mysql-server.git .
//현재 커밋 [Update License Book · mysql/mysql-server@596f0d2 · GitHub](https://github.com/mysql/mysql-server/commit/596f0d238489a9cf9f43ce1ff905984f58d227b6)
```


4. CMake 빌드 환경
```
mkdir build
cd build
cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=../boost
cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=../boost -DCMAKE_CXX_STANDARD=17 (8.4)
```

5. 빌드
```
make -j2
```


### lto-wrapper
```
Linking CXX shared library library_output_directory/libserver_unittest_library.so
c++: fatal error: Killed signal terminated program lto1
compilation terminated.
lto-wrapper: fatal error: /usr/bin/c++ returned 1 exit status
compilation terminated.
```

lto -> linking optimizor에서 메모리를 너무 많이 잡아먹어 프로세스가 강제로 종료된 것으로 추정된다. 다시 수행하니 성공했다.

# 이후 작업
1. mysql user 생성
```
useradd -r -s /bin/false -d /nonexistent -M mysql
```
1. 권한 부여
```
sudo chown -R mysql:mysql /usr/src/mysql/mysql-server/build/data/
sudo chmod -R 755 /usr/src/mysql/mysql-server/build/data/
```
1. datadir 초기화
```
sudo -u mysql /usr/src/mysql/mysql-server/build/runtime_output_directory/mysqld --initialize --datadir=/usr/src/mysql/mysql-server/build/data
```

4. 서버 실행
```
sudo -u mysql mysql-server/build/runtime_output_directory/mysqld --datadir=/usr/src/mysql/data > mysqld.out 2>&1 &
```

5. 서버 실행 시, 인증 피하기