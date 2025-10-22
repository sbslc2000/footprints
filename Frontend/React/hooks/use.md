---
상위 개념: "[[React Hooks]]"
---
# use
use 훅은 Promise를 동기적으로 다루는 것 처럼 사용할 수 있게 해주는 React 내장 훅이다. await 없이도 비동기 데이터를 Suspense를 통해 자동으로 대기 시킬 수 있다.

## 사용법
```ts
import { use } from 'react';

function User({ userPromise }: { userPromise: Promise<User> }) {
  const user = use(userPromise); // Promise resolve될 때까지 suspend
  return <div>{user.name}</div>;
}
```
prop을 통해 promise를 전달받는다. promise는 use 훅에 담기고, 그 반환값을 동기식으로 사용한다.

```ts
// app/page.tsx (Server Component)
import { Suspense } from 'react';
import User from './User';

// 서버에서 비동기 데이터 가져오는 함수
async function getUser() {
  const res = await fetch('https://jsonplaceholder.typicode.com/users/1');
  return res.json();
}

export default function Page() {
  // Promise를 바로 생성하지만 await 하지 않음
  const userPromise = getUser();

  return (
    <Suspense fallback={<div>사용자 정보를 불러오는 중...</div>}>
      <User userPromise={userPromise} />
    </Suspense>
  );
}
```

User 컴포넌트를 사용할 때에는 Suspense로 묶어주어야 한다. user가 가져와지기 전에는 fallback이 보여지며, user가 로딩된 이후에는 정상적으로 User가 보여진다.