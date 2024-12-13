```yaml
mysql:  
  build: ./mysql  
  restart: unless-stopped  
  container_name: app_mysql  
  ports:  
    - "3306:3306"  
  volumes:  
    - ./mysql/mysql_data:/var/lib/mysql  
    - ./mysql/sqls/:/docker-entrypoint-initdb.d/  
  environment:  
      MYSQL_DATABASE: myapp  
      MYSQL_ROOT_PASSWORD: 12345678
```

Docker Volume을 사용하여 데이터베이스에 저장된 자료를 컨테이너를 지우더라도 자료가 지워지지 않게 해줄 수 있다.

일반적인 경우 도커 이미지로 컨테이너를 생성했을 때, DB에 수정된 값들은 컨테이너 안에 저장되며 컨테이너 삭제시 저장된 데이터도 함께 삭제된다. 하지만 위와 같이 volume을 설정해준다면 데이터의 영속성을 보장할 수 있다.

Docker Volume을 통해 도커 컨테이너 내부의 데이터를 Docker Area라고 부르는 호스트 파일 시스템에 저장하게 만든다. 이제는 컨테이너를 삭제했다가 재부팅하더라도 도커 Area의 데이터는 그대로 유지된다.
