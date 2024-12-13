# Google Java Format 적용법

## build.gradle을 통한 코드 포맷 제어
```groovy
plugins {  
    id 'com.diffplug.spotless' version '6.18.0'  
}  
  
spotless {  
    java {  
        googleJavaFormat('1.15.0')  
    }  
}
```

이후 `./gradlew spotlessApply` 를 수행하면 포맷을 조정할 수 있다.

gradle 빌드 시 자동으로 포맷팅을 적용하고 싶다면 다음 내용을 build.gradle에 적용한다.
```groovy
tasks.withType(JavaCompile).configureEach {
    finalizedBy 'spotlessApply' 
}
```
##  intellij와 함께 사용

1. google-java-format plugin 설치
2. Help -> Edit Custom VM Options에 다음 내용 추가 후 IDE 재시작
```
--add-exports=jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED
--add-exports=jdk.compiler/com.sun.tools.javac.code=ALL-UNNAMED
--add-exports=jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED
--add-exports=jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED
--add-exports=jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED
--add-exports=jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED
```