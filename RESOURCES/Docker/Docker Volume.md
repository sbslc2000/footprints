Docker Volume을 사용하지 않는다면 소스코드의 변경에도 [[Docker Image]]를 다시 생성 해줘야 한다.
![[Pasted image 20230922164241.png]]
하지만 Docker Volume을 사용한다면 도커 컨테이너에서 로컬에 있는 파일을 **참조**하여 반영하게 만들 수 있다.
![[Pasted image 20230922164252.png]]

```shell
docker run -p 8080:8080
	-v /usr/src/app/node_modules //node_modules를 매핑 대상에서 제외
	-v $(pwd):/usr/src/app //해당 컨테이너 주소와 로컬을 매핑
	<이미지 아이디>
```

매핑을 시켜놓으면 React npm run start와 같이 실시간으로 reload되는 기능도 도커 컨테이너가 로컬의 파일을 읽어서 리로드한다. 스프링부트에서는 어떻게 사용할까?

### 하위 문서
[[Docker Volume을 이용한 데이터베이스 데이터 유지하기]]