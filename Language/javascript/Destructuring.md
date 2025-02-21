---
상위 링크: "[[Javascript]]"
---
# Destructuring
구조 분해 할당이라고도 부르는 이 문법은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 해준다. Perl이나 Python에서도 지원한다.

## 객체 구조 분해 할당
### 기본 구조
객체를 할당받고, 좌항에는 { } 내부에 해체할 속성을 작성해준다.
```javascript
const user = {
	name: 'Pete',
	age: 3,
};

const { name, age } = user;
```

### 변수명 변경
구조 분해 할당을 위해서는 객체의 속성 이름을 필수로 알아야 하나, 다음과 같은 방법으로 변수명을 변경할 수도 있다.
```javascript
const user = {
	name: 'Pete',
	age: 3,
};

const { name, age: userAge } = user;

console.log(name); // Pete
console.log(userAge); // 3
```

### 기본 값
객체로부터 해체된 값이 `undefined` 임을 대비하여 기본값을 넣을 수도 있다. 이는 요청 DTO의 기본값을 넣어주는데에 유용하다.
```javascript
const user = {
	name: 'Pete',
};

const { name, age = 3 } = user;

console.log(name); // Pete
console.log(userAge); // 3
```

## 배열 구조 분해 할당
### 기본 구조
배열을 할당받고, 좌항에는 \[ ] 내부에 해체할 변수명을 작성해준다. 해체되는 요소는 배열의 순서를 따른다.
```javascript
const list = [1, 2, 3, 4, 5];
const [a, b, c] = list;

console.log(a, b, c); // 1 2 3
```

### 나머지 할당
나머지 구문은 분해하고 남은 부분을 하나의 변수에 할당하게 도와준다.
```javascript
const list = [1, 2, 3, 4, 5];
const [a, b, ...rest] = list;

console.log(a); // 1
console.log(b); // 2
console.log(c); // [3,4,5]
```

### 배열 구조 분해 할당을 응용한 swap
배열 구조 분해 할당을 응용하면 다음과 같이 하나의 표현식만으로 두 변수의 값을 교환할 수 있다.
```javascript
let a = 3;
let b = 5;

[a, b] = [b, a];

console.log(b); // 3
console.log(a); // 5
```