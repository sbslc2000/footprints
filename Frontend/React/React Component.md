---
상위 개념: "[[React]]"
---
# React Component
```jsx
export default function FeedCard() {
	
	return (
		<card>
			<div>title</div>
			<div>content</div>
		</card>
	);
}
```

리액트의 컴포넌트는 위와 같이 함수 형태이며, jsx라는 특수한 문법을 통해 작성됩니다. JSX 문법은 마치 HTML 태그를 작성하는 것 처럼 보이지만, 사실은 HTML 태그처럼 사용하게 만들어주는 특수한 문법으로 JS위에서 동작합니다. 따라서 몇가지 제약 사항을 인지하고 있을 필요가 있습니다.

1. `{ }` 내부에는 자바스크립트 코드를 작성할 수 있다.

```jsx
export default function FeedCard() {

	const title = '기깔난 React 강의';
	
	return (
		<card>
			<div>{title}</div>
			<div>1 + 1 = { 1 + 1 } </div>
		</card>
	);
}
```

위 코드 중 `<div>1 + 1 = { 1 + 1 } </div>` 에 먼저 집중해 봅시다. 앞에 나오는 `1 + 1 =` 은 정적(사전에 결정되어 바뀌지 않음)인 부분입니다. 이들은 HTML의 내부 값으로써 그대로 렌더링됩니다. 뒤에 나오는 `{ 1 + 1 }` 은 이 내부의 식이 자바스크립트임을 표현합니다. 자바스크립트의 1 + 1이 결과는 2이므로, 최종적으로 다음과 같은 엘리먼트가 렌더링됩니다.

```html
<div>1 + 1 = 2</div>
```

같은 원리로 `<div>{title}</div>`의 `{title}`은 title이 일반 문자열이 아닌 자바스크립트라는 것을 의미합니다. title은 '기깔난 React 강의'를 담고 있는 변수이므로, 최종적으로 다음 엘리먼트가 렌더링됩니다.

```html
<div>기깔난 React 강의</div>
```

2. class 대신 className을

JSX에서 다루는 HTML 태그는 사실 그렇게 보이도록 만든 자바스크립트 코드라고 말씀드렸습니다. 이에 따라 일반적으로 HTML을 다룰 때에 사용하는 방법에 약간의 제약이 있습니다.

```html
return <div className='greeting'>hello</div>
```

class는 자바스크립트 예약어이기 때문에, jsx내에서 사용할 수 없습니다. 대신 className을 사용하여 태그에 class 속성을 지정해줄 수 있습니다.

3. 스타일 속성은 객체로

```html
<div style="color: red; background-color: blue;">
```

HTML을 작성할 때 인라인 스타일은 위와 같이 작성합니다.

```jsx
<div style={{ color: 'red', backgroundColor: 'blue' }} />
```

하지만 JSX에서는 위와 같이 객체 형태로 표현해야하며, 속성명은 카멜 케이스로 작성되어야 합니다.

4. 태그는 항상 닫아야하며, 하나의 거대한 태그로 묶여있어야 함.

```jsx
<input >
```

위와 같이 열린 태그로 HTML이 작성되고 닫히지 않는 것을 HTML은 용인합니다. 에러가 나지 않습니다. 하지만 JSX에서 위와 같이 작성한다면 에러가 발생합니다.

```jsx
return (
	<div>Royal</div>
	<div>Milktea</div>
); (x)

return (
	<div>
		<div>Royal</div>
		<div>Milktea</div>
	</div>
);
```

또한 두 개 이상의 태그가 나란히 반환되는 것은 허용되지 않습니다. 내부에 여러 태그가 있는 것은 괜찮지만, 최종적으로 하나의 태그로 묶여있어야 합니다.