---
상위 개념: "[[../Javascript|Javascript]]"
---
# innerHTML과 innerText의 차이점

## 반환 값
innerHTML은 해당 태그의 내부에 있는 HTML 전체를 가져오지만, innerText의 경우 내부의 태그를 전부 제거하고 사용자에게 보이는 문자만 반환한다.

```html
<div id="box">Hello <strong>World</strong></div>
```

```javascript
const box = document.getElementById("box");

console.log(box.innerHTML); //'Hello <strong>World</strong>'
console.log(box.innerText); // 'Hello, World'
```

## 값 대입 시
```javascript
box.innerHTML = '<em>Hi</em>'
```
위 경우 box의 내부 요소는 주어진 엘리먼트로 대체된다.
```html
<div id='box'><em>Hi</em></div>

```

```javascript
box.innerText = '<em>Hi</em>'
```
innerText를 사용하는 경우 주어진 값이 문자로 취급되어 box 내에 들어간다.