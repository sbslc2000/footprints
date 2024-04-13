---
상위 링크: "[[Actuator]]"
---
# 엔드포인트 설정
엔드포인트를 사용하려면 다음 2가지 과정이 모두 필요하다.
1. 엔드포인트 활성화
2. 엔드포인트 노출

엔드포인트를 활성화한다는 것은 해당 기능 자체를 사용할지 말지를 결정하는 것이다.
엔드포인트를 노출한다는 것은 활성화된 엔드포인트를 HTTP에 노출할지 아니면 JMX에 노출할지 등을 선택하는 것이다.

엔드포인트는 대부분 기본으로 활성화되어있다. 노출이 되어있지 않을 뿐이다. (shutdown 제외) 따라서 어떤 엔드포인트를 노출할지 선택하면 된다.

### 모든 엔드포인트를 웹에 노출
```yaml
management:
	endpoints:
		web:
			exposure:
				include: "*"
```

### 엔드포인트 활성화
```yaml
 management:
	 endpoint:
		 shutdown:
			 enabled: true
	 endpoints:
		 web:
			 exposure:
				 include: "*"
```
shutdown 엔드포인트를 활성화한 경우 /actuator/shutdown을 post로 호출하면 서버를 종료시킬 수 있다.

# 다양한 엔드포인트
각각의 엔드포인트를 통해서 개발자는 애플리케이션 내부의 수 많은 기능을 관리하고 모니터링할 수 있다.

* beans : 스프링 컨테이너에 등록된 스프링 빈을 보여준다.
* conditions: condition을 통해서 빈을 등록할 때 평가 조건과 일치하거나 일치하지 않는 이유를 표시한다.
![](https://i.imgur.com/wrblsOc.png)
* configprops : @ConfigurationProperties를 보여준다.
* env : Environment 정보를 보여준다.
* health : 애플리케이션 헬스 정보를 보여준다.
* httpexchanges : HTTP 호출 응답 정보를 보여준다. HttpExchangeRepository를 구현한 빈을 별도로 등록해야 사용할 수 있다.
* info : 애플리케이션의 정보를 보여준다.
* loggers : 애플리케이션 로거 설정을 보여주고 변경도 할 수 있다.
* metrics : @RequestMapping 정보를 보여준다.
* threaddump : 쓰레드 덤프를 실행해서 보여준다.
* shutdown : 애플리케이션을 종료한다. 이 기능은 기본으로 비활성화 되어있다.

> [!info] 전체 엔드포인트 목록
> [Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints)

## 헬스 정보
헬스 정보를 사용하면 애플리케이션 에 문제가 발생했을 때 문제를 빠르게 인지할 수 있다.

헬스 정보는 단순히 애플리케이션이 요청에 응답할 수 있는지 판단하는 것을 넘어서 애플리케이션이 사용하는 데이터베이스가 응답하는지, 디스크 사용량에는 문제가 없는지 같은 다양한 정보들을 포함해서 만들어진다.

헬스 정보를 더 자세히 보려면 다음 옵션을 지정하면 된다.
```yaml
 management:
	 endpoint:
		 health:
			 show-details: always
```

result
```json
{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP",
      "details": {
        "database": "H2",
        "validationQuery": "isValid()"
      }
    },
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 494384795648,
        "free": 394251321344,
        "threshold": 10485760,
        "path": "/Users/seobeomseok/project/boot-source-20230228/start/actuator/.",
        "exists": true
      }
    },
    "ping": {
      "status": "UP"
    }
  }
}
```

validationQuery는 JDBC Spec 에서 제공하는 기능으로 DB가 정상적으로 응답할 수 있는지 확인할 수 있는 쿼리이다. 더미 쿼리를 직접 만들 필요 없이 이 기능을 사용하면 체크하기 편하다.

위와 같은 요소 중 하나라도 실패한다면, 최종적인 status는 "down"이 된다.

만약 드러나는 정보가 너무 많다면 다음과 같은 설정을 통해 상태값만 확인할 수 있다.
```yaml
management:
	endpoint:
		health:
			show-components: always
```

result
```json
 {   
 "status": "UP",
   "components": {
     "db": {
       "status": "UP"
     },     
     "diskSpace": {
       "status": "UP"
     },
     "ping": {
       "status": "UP"
    } 
  }
}
```

추가적으로 db, mongo와 같은 수 많은 컴포넌트들의 헬스 기능을 기본으로 제공한다. 자세한 지원 기능은 다음 공식 매뉴얼을 참고할 수 있다.
[Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints.health.auto-configured-health-indicators)

다음과 같은 방법으로 직접 헬스 기능을 구현해서 추가할 수 있다.
[Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints.health.writing-custom-health-indicators)

## 애플리케이션 정보
info 엔드포인트는 애플리케이션의 기본 정보를 노출한다.

기본으로 제공하는 기능은 다음과 같다.
* java : 자바 런타임 정보
* os : OS 정보
* env : Environment에서 info로 시작하는 정보
* build : 빌드 정보, `META-INF/build-info.properties` 파일을 필요로 한다.
* git : git 정보, `git.properties` 파일이 필요하다.

env, java, os는 기본으로 비활성화 되어있다. 활성화하는 방법은 다음과 같다. management.endpoint.info 가 아님에 주의하자.

java, os 활성화하기
```yaml
management:
	info:
		java:
			enabled: true
		os:
			enabled: true
			
```

result
```json
{  
	"java":{
       "version":"17.0.3",
       "vendor":{
          "name":"JetBrains s.r.o.",
          "version":"JBR-17.0.3+7-469.37-jcef"
       },
       "runtime":{
          "name":"OpenJDK Runtime Environment",
          "version":"17.0.3+7-b469.37"
		}, 
		"jvm":{
          "name":"OpenJDK 64-Bit Server VM",
          "vendor":"JetBrains s.r.o.",
          "version":"17.0.3+7-b469.37"
          } 
    },
    "os":{
       "name":"Mac OS X",
       "version":"12.5.1",
       "arch":"aarch64"
    }
}
```

env 정보를 보려면 다음과 같은 설정을 추가해야한다.
```yaml
management:
	info:
		env:
			enabled: true

info: 
	app:
		name: hello-actuator
		company: yh
```
info를 추가한 이유는 응답에서 확인하기 위함이다. 결과로 application.yml에서 info로 시작하는 부분의 정보가 노출됨을 확인할 수 있다.

result
```json
{  
	"app":
	{
		"name":"hello-actuator",
		"company":"yh"
    }
	... 
}
```

build 정보를 노출하기 위해서는 빌드 시점에 META-INF/build-info.properties 파일을 만들어야 한다. 이는 직접 만들 필요 없고, gradle에 다음 내용을 추가해주면 된다.

```groovy
springBoot {
	buildInfo()
}
```

이후 빌드를 해보면 build 폴더 안에 resources/main/META-INF/build-info.properties를 확인할 수 있다.
```
 build.artifact=actuator
 build.group=hello
 build.name=actuator
 build.time=2023-01-01T00\:000\:00.000000Z
 build.version=0.0.1-SNAPSHOT
```

build는 기본으로 활성화 되어있기 때문에 이 팔일만 있다면 바로 확인할 수 있다.

git
앞서본 build와 유사하게 빌드 시점에 사용한 git 정보도 노출할 수 있다. git 정보를 노출하려면 git.properties 파일이 필요하다.

```groovy
plugins { ...
     id "com.gorylenko.gradle-git-properties" version "2.4.1" //git info
 }
```

git.properties에는 다음과 같은 정보들이 담기며 info를 통해 이 정보를 확인할 수 있다. 
```java
 git.branch=main
 git.build.host=kim
 git.build.user.email=zipkyh@mail.com
 git.build.user.name=holyeye
 git.build.version=0.0.1-SNAPSHOT
 git.closest.tag.commit.count=
 git.closest.tag.name=
git.commit.id=754bc78744107b6423352018e46367f5091b181e
 git.commit.id.abbrev=754bc78
 git.commit.id.describe=
 git.commit.message.full=fitst commit\n
 git.commit.message.short=fitst commit
 git.commit.time=2023-01-01T00\:00\:00+0900
 git.commit.user.email=zipkyh@mail.com
 git.commit.user.name=holyeye
 git.dirty=false
 git.remote.origin.url=
git.tags=
git.total.commit.count=1
```

> [!info] info 사용자 정의 기능 추가
> [Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints.info.writing-custom-info-contributors)


## 로거
loggers 엔드포인트를 호출해보자.
{host}/actuator/loggers
![](https://i.imgur.com/tSi9Q72.png)

특정 파트만 검색하고자 한 경우 다음과 같이 검색할 수 있다.
{host}/actuator/loggerse/hello.controller

### 실시간 로그 레벨 변경
개발 서버는 보통 DEBUG 로그를 사용하지만, 운영 서버는 보통 요청이 아주 많다. 따라서 로그도 너무 많이 남기 때문에 DEBUG 로그까지 모두 출력하게 되면 성능이나 디스크에 영향을 주게 된다. 따라서 운영서버는 중요하다고 판단되는 INFO 로그 레벨을 사용하는 것이 일반적이다.

그런데 서비스 운영 중에 문제가 있어서 급하게 DEBUG나 TRACE 로그를 남겨서 확인하고 싶다면 어떻게 해야할까? loggers 엔드포인트를 사용하면 애플리케이션을 다시 시작하지 않고, 실시간으로 로그 수준을 변경할 수 있다.

HTTP \[POST]
{host}/actuator/loggers/hello.controller 
(body)
{
	"configuredLevel" : "TRACE"
}

이렇게 변경한 사항은 당연하지만 서버를 리로딩하면 원상복구된다.


## HTTP 요청 응답 기록
HTTP 요청과 응답의 과거 기록을 확인하고 싶다면 httpexchanges 엔드포인트를 사용하면 된다

HttpExchangeRepository 인터페이스의 구현체를 빈으로 등록하면 httpexchanges 엔드포인트를 사용할 수 있다.

스프링부트는 기본적으로 `InMemoryHttpExchangeRepository` 구현체를 제공한다. 이 구현체는 최대 100개의 http 요청 기록을 제공하며, 개수가 넘어가면 과거 요청을 삭제한다. setCapacity()로 최대 요청 수를 변경할 수 있다.

{host}/actuator/httpexchanges
result
```json
{
      "timestamp": "2024-04-07T13:53:19.994209Z",
      "request": {
        "uri": "http://localhost:8080/log",
        "method": "GET",
        "headers": {
          "host": [
            "localhost:8080"
          ],
          "connection": [
            "keep-alive"
          ],
          "cache-control": [
            "max-age=0"
          ],
          "sec-ch-ua": [
            "\"Google Chrome\";v=\"123\", \"Not:A-Brand\";v=\"8\", \"Chromium\";v=\"123\""
          ],
          "sec-ch-ua-mobile": [
            "?0"
          ],
          "sec-ch-ua-platform": [
            "\"macOS\""
          ],
          "dnt": [
            "1"
          ],
          "upgrade-insecure-requests": [
            "1"
          ],
          "user-agent": [
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36"
          ],
          "accept": [
            "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7"
          ],
          "sec-fetch-site": [
            "none"
          ],
          "sec-fetch-mode": [
            "navigate"
          ],
          "sec-fetch-user": [
            "?1"
          ],
          "sec-fetch-dest": [
            "document"
          ],
          "accept-encoding": [
            "gzip, deflate, br, zstd"
          ],
          "accept-language": [
            "ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7,th;q=0.6,fil;q=0.5"
          ]
        }
      }
```

이 기능은 매우 단순하고 기능에 제한이 많기 때문에 개발 단계에서만 사용하고, 실제 운영 서비스에서는 모니터링 툴이나 핀포인트, Zipkin 같은 다른 기술을 사용하는 것이 좋다.
