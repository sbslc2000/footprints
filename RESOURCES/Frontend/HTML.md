# HTML
HTML은 웹 페이지의 표시를 위해 개발된 마크업 언어이다
## 구성요소
- **Tag**
    - 웹 문서를 구성하는 명령어
    - 꺽새 안에 들어있는 정보를 정의하는 형식
    - 요소의 일부로, 시작태그와 종료태그 두 종류가 있음
    - 종료태그가 없는 <br /> 과 같은 태그도 존재함. → 콘텐츠가 필요 없는 경우
- **Element**
    - 시작태그와 종료태그, 그 사이의 내용으로 구성
    - `Empty Element` : 내용 없이 구조적인 기능만 하는 요소
    - `Block Element`
        - 다른 Block Element 와 Inline Element를 포함할 수 있음
        - Block Element 이후에 Block Element를 사용하면 아랫 줄에 나타남
        - margin 과 padding, width, height 값을 가질 수 있음
    - `Inline Element`
        - 다른 Block Element 안에 포함되어 있음
        - 다른 Inline Element를 포함할 수 잇음
        - 그러나 다른 Block Element를 포함할 수는 없다.
        - width와 height를 가질 수 없다.
        - css의 속성 display:block을 통해서 Block Element와 같이 사용될 수 있다.
- **Attribute**
    - 태그를 보조하는 명령어로 태그 안쪽에 있다.
    - 태그의 문법 명령어가 다루지 못하는 명령을 보조합니다.
    - width, height, alt, style, href 등이 있다.
## Semantic Element
Semantic이란 의미론이란 뜻으로, **Semantic Element는 의미가 있는 요소**를 말하며 브라우저와 개발자 모두에게 의미를 명확하게 설명하는 기능을 한다.
`<div> <span>` 과 같은 태그는 내용에 대해 아무 것도 알려주지 않지만, `<form>, <article>, <table>` 과 같은 태그들은 그 내용이 어떠한 내용인지 유추할 수 있게 해준다.
![[Pasted image 20230923101335.png]]
- header : 주로 제목과 소개 내용을 포함한다.
- section : 일반적으로 제목이 있는 주제별 콘텐츠 그룹을 의미한다.
- nav : 주로 목차, 메뉴 등에 사용된다.
- footer : 문서의 바닥글이며, 저작권 정보, 사이트 맵등을 포함한다.
- article : 사이트 안에서 독립적으로 구분해 배포하거나 재사용할 수 있는 구획을 나타낸다.

## HTML 기본 구조

```html
<!DOCKTYPE html>
<html lang="kr">
<head>
	<title>My Document</title>
</head>
<body>
</body>
</html>
```

- `DOCTYPE`
    - html 문서 맨 처음에 명시에서 이 문서의 버전을 나타낸다.
    - <!DOCTYPE html> 은 html5 버전임을 나타낸다.

asdf