---
상위 링크: "[[Docker]]"
---
 컨테이너 간에는 환경이 분리되어있기 때문에 별도의 설정 없이는 통신할 수 없습니다. Docker Compose는 다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구입니다. Docker Compose를 통해 멀티 컨테이너 상황에서 쉽게 네트워크를 연결시켜줄 수 있습니다.

## Terminologies
* **Services** : 컨테이너 집합을 이르는 말

## docker-compose.yml
```yaml
version: "3"  
services:  
  redis-server(컨테이너 별명):  
    image: <BASE IMG 이름>
  node-app:  
    build : .  
	    context: <도커 이미지를 구성하기 위한 파일과 폴더가 있는 위치>
	    dockerfile : <도커파일을 지정, 기본값은 dockerfile>
    ports:  
      - "8000:8080"
	volumes: <로컬에 있는 파일 매핑
		- /usr/src/app/node_modules <제외할 디렉터리>
		- ./:/usr/src/app <매핑할 디렉터리 로컬:컨테이너>
```
* version : Docker Compose의 버전
* services : 조합할 컨테이너 목록과 정보
* redis-server, node-app : 컨테이너 이름
* image : 사용할 base image
* build : 해당 디렉터리에 node-app의 dockerfile이 있음으 ㄹ알림
* ports : 포트를 매핑함 (8000: 외부포트 , 8080: 내부포트)

## Docker Compose 관련 명령어
### 실행하기
```shell
docker-compose up
```
docker-compose 파일이 있는 디렉터리에서 위 명령어를 수행해주면 된다. 이미지가 없다면 이미지를 빌드하고 컨테이너를 구동한다.
* --build : 새롭게 이미지를 빌드하여 수정된 내용을 반영한다. 수정된 내용이 없더라도 이미지를 새롭게 빌드한다.
### 종료하기
```shell
docker-compose down
```
터미널의 같은 위치에서 다음 명령어를 입력하면 된다.
하지만 docker-compose up을 사용한다면 해당 터미널이 컨테이너측의 입력을 받기 때문에, 종료를 시키기 위해서는 별도의 터미널로 들어가야 한다. 이것이 귀찮다면 -d 속성을 제공하여 컨테이너를 백그라운드에 실행시킬 수 있다.
```shell
docker-compose up -d
```