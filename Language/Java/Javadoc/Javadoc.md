---
상위 링크: "[[Java]]"
---
# Javadoc

Javadoc은 JDK와 함께 패키지로 제공되는 도구이다. JDK가 설치되어있다면 Javadoc을 사용할 수 있으며, Java 소스 코드의 코드 문서를 생성하는데에 도움을 주는 도구이다.

![](https://i.imgur.com/X3HNXix.png)

## Javacod 사용 방법
cmd 창을 열고 java 파일이 있는 디렉토리로 이동한 후 javadoc -d doc \*.java 로 생성할 수 있다.

## Javadoc의 대상이 되는 주석 작성방법

### 주석 형태
Javadoc 문서 생성의 대상으로하는 경우에는 다음과 같은 방법으로 작성해야한다.
```java
/**
	주석
	주석
*/

/** 주석 */
```

### 주석의 구성
```java
/** 
* 설명문 영역입니다. 
* 여러 줄로 작성 할 수 있습니다. 
* Html문으로 해석됨으로 <br> 등 Html태그 이용이 가능합니다. 
* <a href="https://gochigo.kr" target="_blank">고치고</a> 
* @version 2.0 
* @author gochigolab 
* @param String 이름 
* @param int 시퀀스 
*/
```

주석 안은 '설명문'과 '태그 섹션' 이렇게 두 가지로 구분할 수 있다.
* 설명문: 클래스 또는 메소드 등에 대해 간략하게 설명한 글이며, 여러 줄을 작성할 수 있다. HTML로 해석되기 때문에 단순히 행을 바꿔 작성하기보다는 명시적으로 \<br>을 사용하여 줄바꿈 해야한다.
* 태그 섹션 : @로 시작하는 태그들이 작성되는 곳이다.