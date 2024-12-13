---
상위 개념: "[[Docker]]"
---
# 실행중인 컨테이너 보기
```shell
docker ps
```
ps: process status
![[Pasted image 20230921113409.png]]
![[Pasted image 20230921113537.png]]
![[Pasted image 20230921113555.png]]

* **CONTAINER ID** : 컨테이너의 고유한 해쉬 값 
* **IMAGE** : 컨테이너 생성 시 사용한 도커 이미지
* **COMMAND** : 컨테이너 시작 시 실행될 명령어
* **STATUS** : 컨테이너의 상태
	* UP (실행 중), Exited (종료), Pause(일시정지)
* **PORTS** : 컨테이너가 개방한 포트와 호스트에 연결한 포트
* **NAMES** : 컨테이너의 고유한 이름이며, --name 속성을 통해 지정하지 않으면 도커 엔진이 임의로 설정
## 원하는 항목만 보기
```shell
docker ps --format 'table{{.Names}}\ttable{{.Image}}'
```
![[Pasted image 20230921115323.png]]
## 모든 컨테이너 나열
```shell
docker ps -a
```
![[Pasted image 20230921115359.png]]
# 생명주기 관련 명령어
## 생성
docker create와 함께 이미지 이름을 제공하면 이미지 파일의 스냅샷이 도커 컨테이너의 디스크에 올라간다. 이후 생성된 컨테이너 ID를 출력함
```shell
docker create <IMAGE NAME>
```
![[Pasted image 20230921120932.png]]
## 시작 
docker start와 함께 컨테이너 ID를 제공하면 이미지의 기본 커맨드가 수행됨. -a 는 attach의 약자로, 컨테이너의 수행으로 인해 생기는 콘솔 output을 출력함.
```shell
docker start <CONTAINER ID/NAME>
docker start -a <CONTAINER ID/NAME>
```
![[Pasted image 20230921121026.png]]
### 생성과 시작을 동시에, run
docker run에 도커 이미지 이름을 제공하면 도커 컨테이너를 생성하고 시작까지 수행해준다.
```shell
docker run <이미지 이름>
```

속성
* **-p <외부포트>:<도커컨테이너포트>** : 포트를 매핑해준다.
* -d : detach의 약자로, 실행만 시키고 터미널을 빠져나온다.
## 중지 
도커의 생명주기를 중지하기 위해서는 docker stop과 docker kill을 사용할 수 있다.
```shell
docker stop <중지할 컨테이너 아이디/이름>
docker kill <중지할 컨테이너 아이디/이름>
```
* **stop** : gracefully 하게 중지시킴 -> 하던 작업들을 완료한 뒤 정지
* **kill** : 바로 컨테이너를 중지
## 삭제
docker rm 을 통해 도커 컨테이너를 삭제할 수 있다.
```shell
//기본 삭제 명령어
docker rm <아이디/이름>

//모든 컨테이너 삭제 : 지워진 도커 컨테이너의 ID를 출력함
docker rm `docker ps -a -q`

//도커 이미지, 네트워크 모두 삭제하고 싶다면?
//실행중인 컨테이너에는 영향을 주지 않는다.
//이후 확보된 영역의 크기를 출력함
docker system prune 
```
# 실행중인 컨테이너에 명령어 전달
exec 명령어를 통해 실행중인 컨테이너에 명령어를 전달할 수 있다.
```shell
docker exec <컨테이너 아이디>
docker exec f3992h4g ls
docker exec -it f3392h4g redis-cli 
```

* **-i** : 상호작용을 한다.
* **-t** : 터미널을 사용한다.
## 실행중인 컨테이너에서 터미널 사용하기
마지막 명령어로 shell 명령어를 사용하면 된다.
```shell
docker exec -it <컨테이너 아이디> sh | bash | zsh | ...
```