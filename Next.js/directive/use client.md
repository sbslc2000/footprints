---
상위 개념: "[[Next.js Directive]]"
---
# use client
```ts
'use client'
 
import { useState } from 'react'
 
export default function Counter() {
  const [count, setCount] = useState(0)
 
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  )
}

```
`use client` 지시어는 컴포넌트가 클라이언트 사이드에서 렌더링되며, 상태 관리, 이벤트 핸들링, 브라우저 API 등 클라이언트 측 상호작용을 할 수 있음을 나타낸다.

## props 제한
'use client' 지시어가 사용된 컴포넌트의 props는 직렬화 가능해야한다. 함수의 경우 기본적으로 직렬화 불가능하기에, 클라이언트 컴포넌트에 전송될 수 없다.

```ts
'use client'
export default function Counter({
  onClick /* ❌ Function is not serializable */,
}) {
  return (
    <div>
      <button onClick={onClick}>Increment</button>
    </div>
  )
}
```

