# 도커 이미지 생성

![](https://i.imgur.com/dfmQ0aI.png)

## Dockerfile 생성
Dockerfile이란 도커 이미지를 만들기 위한 설정 파일이며, 컨테이너가 어떻게 행동해야하는지에 대한 설정들을 정의해 주는 곳이다.
### Dockerfile 만드는 과정
Dockerfile을 만들기 전 도커 이미지에서 필요한 것이 무엇인지를 고려해야한다.
1. 베이스 이미지를 명시해준다. (파일 스냅샷에 해당)
2. 추가적으로 필요한 파일을 다운 받기 위한 몇가지 명령어를 명시해준다. (파일 스냅샷에 해당)
3. 컨테이너 시작 시 실행 될 명령어를 명시해준다. (시작 시 실행 될 명령어에 해당)
> [!info]
> 베이스 이미지란 무엇인가?
> * 도커 이미지는 여러개의 레이어로 되어 있다. 그 중에서 베이스 이미지는 이 이미지의 기반이 되는 부분이다.
> * 레이어는 중간 단계의 이미지이다.
> ![[Pasted image 20230921165540.png]]

### dockerfile 작성
```shell
# 베이스 이미지를 명시해준다.
FROM baseImage

# 추가적으로 필요한 파일들을 다운로드 받는다.
RUN command

# 컨테이너 시작 시 실행될 명령어를 명시해준다.
CMD [ "executable" ]
```

* **FROM** : 이미지 생성 시 기반이 되는 베이스 이미지를 지정
	* <이미지 이름>:<태그> 형식으로 작성
	* 태그를 안붙이면 자동적으로 가장 최신것으로 다운받음
* **RUN** : 도커이미지가 생성되기 전에 수행할 쉘 명령어
* **CMD** : 컨테이너가 시작되었을 때 실행할 파일 또는 쉘 스크립트
	* DockerFile 내에서 1회만 쓸 수 있음

```shell
##베이스 이미지 설정  
FROM node:10  
  
## 프로젝트 소스코드가 들어갈 디렉토리를 지정  
WORKDIR /usr/src/app  
  
## 프로젝트 소스코드의 루트에 있는 모든 파일을  
## 워크디렉토리로 이동  
COPY package.json ./  
  
## npm install을 수행  
RUN npm install  
  
## 이렇게 해주면 소스코드가 변할 때 종속성을 다시 설치하지 않아도 됨  
COPY ./ ./  
  
## 프로그램의 entry point를 지정  
CMD ["node", "server.js"]
```


### 빌드
build 명령어와 함께 dockerfile이 있는 디렉토리를 명시해준다.
```docker
docker build <path>
```
![[Pasted image 20230921171041.png]]
![[Pasted image 20230921171230.png]]
![[Pasted image 20230921171239.png]]
먼저 베이스 이미지를 통해 임시 컨테이너를 생성하고, 이것을 토대로 새로운 레이어(종속성, 커맨드)들이 적용되어 새로운 이미지를 만든다.
![[Pasted image 20230921171834.png]]
### 이미지에 이름 설정하기
이미지에 이름을 설정하려면 빌드 시 옵션을 제공하면 된다.
```shell
docker build <디렉터리> -t <나의 아이디>/<저장소 or 프로젝트이름>:<버전>
docker run <나의 아이디>/<저장소 or 프로젝트이름>
```

## Stage

* Builder stage
	* 빌드파일들을 생성하는 부분이다.
* Run stage
	* 빌드파일들을 실행시키는 부분이다.
```dockerfile
# 이 스테이지에 builder라는 별칭을 붙여준다.
FROM node:alpine as builder  
WORKDIR /usr/src/app  
COPY package.json ./  
RUN npm install  
COPY ./ ./  
RUN npm run build  

# nginx 이미지를 기반으로 새로운 스테이지를 시작한다.
FROM nginx  
#builder 스테이지로부터 파일을 복사하여 가져옴
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
```

1. **빌드와 배포의 단계 분리**: 첫 번째 스테이지(`builder`)에서는 애플리케이션의 빌드를 위한 모든 도구와 의존성을 포함하게 됩니다. 하지만 최종 이미지에는 이러한 도구와 의존성이 필요하지 않습니다. 따라서 두 번째 스테이지에서는 단순히 빌드 결과물만을 Nginx에 복사하여 불필요한 크기 증가를 방지합니다.
2. **이미지 크기 최소화**: 빌드에 필요한 도구와 의존성을 포함한 이미지는 크기가 크게 됩니다. 멀티 스테이지 빌드를 사용함으로써 최종 이미지의 크기를 최소화할 수 있습니다.