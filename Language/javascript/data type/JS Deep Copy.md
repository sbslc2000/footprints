# JS Deep Copy
간단하게 깊은 복사르 처리할 수 있는 방법 중 하나는 JSON으로 직렬화 후 역직렬화하여 객체로 만드는 것이다.
```js
function copyObjectViaJSON(target) {
	return JSON.parse(JSON.stringify(target));
}
```

다른 방법으로는 타겟 혹은 타겟의 속성이 기본형인 경우 그대로 복사하며, 참조형이라면 그 내부의 프로퍼티들을 복사하게 만드는 방법이 있다.
```js
function copyObjectDeep(target) {
	var result = {};
	if (typeof target === 'object' && target !== null) {
		for (var prop in target) {
			result[prop] = copyObjectDeep(target[prop]);
		}
	} else {
		result = target;
	}
	
	return result;
}
```