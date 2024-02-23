---
상위 링크: "[[Test]]"
---
# Jacoco
![](https://i.imgur.com/GZzok0v.png)
[EclEmma - JaCoCo Java Code Coverage Library](https://www.eclemma.org/jacoco/)
JaCoCo는 EclEmma 팀이 만든 무료 Java Code Coverage를 체크하는 라이브러리이다.

## 설정 방법
build.gradle에 플러그인을 추가함으로써 JACOCO를 사용할 수 있다.

### Test Report 보기
```groovy
plugins {  
    id 'jacoco'  
}

//test 이후 jacoco 레포트 생성
tasks.named('test') {  
    useJUnitPlatform()  
    finalizedBy 'jacocoTestReport'  
}

// jacoco 설정  
jacoco {  
    // JaCoCo 버전  
    toolVersion = "0.8.7"  
  
    //  테스트결과 리포트를 저장할 경로 변경  
    //  default는 "$/jacoco"    //  reportsDir = file("$buildDir/customJacocoReportDir")}  
  
/**  
 * 바이너리 커버리지 결과를 사람이 읽기 좋은 형태의 리포트로 저장합니다.  
 * html 파일로 생성해 사람이 쉽게 눈으로 확인할 수도 있고,  
 * SonarQube 등으로 연동하기 위해 xml, csv 같은 형태로도 리포트를 생성할 수 있습니다.  
 */jacocoTestReport {  
    afterEvaluate {  
        classDirectories.setFrom(files(classDirectories.files.collect {  
            fileTree(dir: it,  
                    exclude: [ // 필터링할 파일 목록  
                               '**/dto/**',  
                               '**/Q*',  
                               'goorm/eagle7/stelligence/api/**',  
                    ])  
        }))  
    }  
    reports {  
        html {  
            enabled true  
        }  
    }}
```

이후 테스트를 수행하면 build 하위 폴더에 다음과 같은 디렉터리가 생기며 결과를 html로 확인할 수 있다. 이 디렉터리 정보는 reports의 속성 변경을 통해 커스터마이징 가능하다.
![](https://i.imgur.com/ygzrmbo.png)

이후 프로그램, 패키지, 클래스, 메서드, 라인을 기준으로 하는 커버리지 퍼센트를 html을 열어 확인할 수 있다.
![](https://i.imgur.com/04p7YWb.png)

[Gradle 프로젝트에 JaCoCo 설정하기 | 우아한형제들 기술블로그](https://techblog.woowahan.com/2661/)