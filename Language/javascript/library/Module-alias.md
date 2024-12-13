---
상위 링크: "[[Javascript]]"
---
# Module-alias
Module-alias는 자바스크립트 라이브러리로, Node.js에서 상대주소를 절대주소로 변환하여 레퍼런스할 수 있게 도와준다.

상대주소 기반으로 import하기 위해서는 다음과 같은 문제가 발생한다.
```javascript
const module = require('../../../../some/very/deep/module')
```
이러한 레퍼런스는 보기에 안좋을 뿐 더러 파일 디렉터리에 의존적이므로 위치 변경에 취약하다.

module-alias는 다음과 같이 import할 수 있게 해준다.
```javascript
require('@src/some/very/deep/module')
```

### 참고 링크
[module-alias - npm](https://www.npmjs.com/package/module-alias)