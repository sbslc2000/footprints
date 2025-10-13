# Next.js Metadata
Next.js의 Metadata 기능은 페이지의 \<head> 영역을 구조적으로 선언하는 방식으로 사용하며, SEO, OG, Twitter Card 등을 관리할 수 있게 해준다.

## 정적 메타데이타
`export const metadata = { }`를 page.ts에 선언하는 방식으로 사용한다. Next.js는 빌드할 때 이 객체를 \<head> 태그로 만들어 html에 넣어둔다.
```ts
// app/page.tsx
export const metadata: Metadata = {
	title: "Hello",
	description: "Hello, I'm Pete",
	openGraph: { ... },
}
```

## 동적 메타데이타
페이지의 내용에 따라 메타데이터가 바뀌어야 한다면, `generateMetadata` 함수를 사용할 수 있다. 런타임에 페이지 요청이 들어온 순간 해당 함수는 실행되고, 이 결과물에 따른 head 태그를 만들어 사용자에게 전달된다.
```js

export async function generateMetadata({ params }) {

	const title = async getTitle()
	return {
		title,
	}
}
```

## 메타데이터 병합 규칙
루트 레이아웃 -> 하위 레이아웃 -> 페이지 순으로 병합된다. 가장 깊은 페이지가 우선권을 가지며, 상위 메타데이터는 기본값으로 취급된다.
