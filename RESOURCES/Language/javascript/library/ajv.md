---
상위 링크: "[[Javascript]]"
공식 링크: https://ajv.js.org/
---
# AJV
Ajv는 여러 자바스크립트 환경에서 사용되는 라이브러리로, json schema에 대한 복잡한 유효성 검증 로직을 단순화해준다.

```
const Ajv = require("ajv")
const ajv = new Ajv()

const schema = {
  type: "object",
  properties: {
    foo: {type: "integer"},
    bar: {type: "string"}
  },
  required: ["foo"],
  additionalProperties: false
}

const data = {foo: 1, bar: "abc"}
const valid = ajv.validate(schema, data)
if (!valid) console.log(ajv.errors)
```
