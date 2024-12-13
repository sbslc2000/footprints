---
공식 링크: https://webpack.kr/
상위 링크: "[[Javascript]]"
---
# Webpack
Webpack은 Bundler의 일종이며, 각종 플러그인과 옵션을 사용하여 코드에 대해 다양한 방법으로 변환 및 압축을 수행한다. 이를 통해 애플리케이션의 로딩 및 실행속도를 향상시킬 수 있으며, 난독화를 통해 다른 사용자들이 프로그램 소스코드를 해석학시 어렵게 만든다.

## Webpack 사용법
1. 소스 저장소 분리하기
	1. src 폴더를 만들고 .js파일들을 모두 이동
2. 프로젝트에 웹팩 설치
```
npm install webpack webpack-cli 
```
3. 웹팩 설정 파일 작성
```javascript
//webpack.config.js 
const path = require('path'); 
module.exports = {
	entry: './src/main.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist'), 
	}, // 💡 추가설정들 
	watch: true, // 파일 수정 후 저장시 자동으로 다시 빌드 
	experiments: { 
		topLevelAwait: true // 모듈 await 가능하도록 
	}
};
```
4. 빌드 명령 추가
```
"scripts": { "build": "webpack" },
```
5. 실행
```
npm run build
```

이를 통해 dist 내에는 압축된 소스코드가 존재하게 된다. 이 압축 과정에서 변수 등을 조작하여 클라이언트 코드를 분석하게 어렵게 만든다. 이 내용들은 beautifuler에서 파싱하여 확인할 수 있다.