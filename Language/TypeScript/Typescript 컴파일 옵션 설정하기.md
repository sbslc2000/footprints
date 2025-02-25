---
상위 링크: "[[Typescript]]"
---
# Typescript 컴파일 옵션 설정
TS의 컴파일 옵션은 프로젝트마다 설정할 수 있으며, tsc를 통해 기본 설정이 되어있는 설정파일을 만들 수 있다.

```json
//tsconfig.json
{  
  "compilerOptions": {  
    "module": "CommonJS",  
    "esModuleInterop": true,  
    "noImplicitReturns": true,  
    "sourceMap": true,  
    "strict": true,  
    "target": "es2017",  
    "allowJs": true,  
    "outDir": "lib",  
    "rootDir": "src",  
    "noImplicitAny": false,  
    "skipLibCheck": true,  
    "noUnusedLocals": false,  
    "noUnusedParameters": false,  
    "baseUrl": "src",  
    "inlineSources": true,  
    "paths": {  
      "@/*": ["app/*"]  
    },  
  },
}
```

* module: 모듈 시스템을 지정한다.
* target: 컴파일 결과물의 버전을 지정한다.
* esModuleInterop: CommonJS 모듈과 ES 모듈간 호환여부를 결정한다.
* allowJs: 프로젝트 내에서 .js 파일도 함께 컴파일할지를 결정한다.
* strict: 엄격한 타입검사를 활성화한다. 
* noImplicitReturns : true로 설정한다면 모든 함수에서 명확하게 return을 작성해주어야 한다.
* noImplicitAny: true로 설정한다면, 명시적으로 타입을 지정하지 않은 변수의 타입을 any로 자동지정하는 것을 금지한다.
* noUnusedLocals: true로 설정한다면 사용하지 않는 지역 변수가 존재할시 컴파일 에러를 발생시킨다.
* noUnusedParameters: true로 설정하면 사용하지 않는 파라미터가 존재할 시 컴파일 에러를 발생시킨다.

* outDir: 컴파일 결과물이 위치할 디렉터리를 지정한다.
* rootDir: 소스파일이 위치하는 루트 디렉터리를 지정한다.
* sourceMap: true로 설정한다면 .map 파일을 생성하여 디버깅 시 원본 TS 코드와 연결하게 한다.

* skipLibCheck: true로 설정한다면, node_modules 등에 존재하는 외부 라이브러리에서 발생한 타입 에러를 무시한다.