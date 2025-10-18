---
상위 개념: "[[Next.js]]"
---
# Next.js Project Organization

## Colocation
Next.js는 관련 있는 파일들을 근처에 위치시키는 colocation 철학을 갖는다. `/app` 하위의 디렉터리 구조는 URL과 매핑되지만, 해당 위치에 라우팅과는 관련이 없고, 특정 페이지의 로직이나 레이아웃과 관련있는 파일 역시 놓을 수 있다.

1. 오직 page.js(jsx,ts,tsx)와 route.js(jsx,ts,tsx)만이 url에 매핑된다.
	1. 즉, /app/components/Hello.tsx는 url에 매핑되지 않는다.
2. 라우팅과 관련없는 폴더들은 `_`을 붙여 명시적으로 관리할 수 있다.
	1. 즉, `/app/_components` 하위의 파일들은 심지어 파일명이 page.js 여도 라우팅에 관여하지 않는다.

## Routing Groups
라우팅에 관여하지 않으면서 디렉토리를 분리할 수 있다.
![](https://i.imgur.com/tdNZqnK.png)
이 경우 (admin)이나 (marketing)은 라우팅에 관여하지 않는다.
