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
```


4. CMake 빌드 환경
```
mkdir build
cd build
cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=../boost
```

5. 빌드
```

```