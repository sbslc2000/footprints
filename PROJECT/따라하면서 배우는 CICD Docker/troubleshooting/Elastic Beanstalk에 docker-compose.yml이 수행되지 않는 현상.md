# Issue

Docker Hub에 이미지를 올려놓고 Elastic Beanstalk에서 이를 읽어들여 환경에서 실행될 수 있게 하고자 했는데, docker-compose 실행 중 오류가 생겼다.

# Cause

이는 Docker Build 시점에 애플 실리콘 칩 기반의 시스템에서는 linux/ard64 기반의 이미지를 생성하기 때문에, linux/amd64 기반으로 동작하는 aws 리눅스에서는 사용될 수 없었기 때문이다.

```shell
react The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
```

# Solution

빌드하는 과정에서 linux/amd64에서 수행될 수 있도록 속성을 변경해주었다.

```shell
docker build --platform linux/amd64 -t peteseo/docker-react-app .
```

