# gradle에서 변수 설정
ext로 설정한 변수를 가져올 때에는 따옴표가 아닌 쌍따옴표에 값을 넣어줘야 하는 것 같다.

```groovy
implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"  
annotationProcessor "com.querydsl:querydsl-apt:${queryDslVersion}"

implementation 'com.querydsl:querydsl-jpa:${queryDslVersion}' //이건 안됨
```

# QueryDsl 연동
[[Spring] QueryDSL 완벽 이해하기](https://velog.io/@jkijki12/Spring-QueryDSL-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

