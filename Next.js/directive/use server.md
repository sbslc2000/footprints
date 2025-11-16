---
상위 개념: "[[Next.js Directive]]"
---
# use server
```ts
'use server'
import { db } from '@/lib/db' // Your database client
 
export async function createUser(data: { name: string; email: string }) {
  const user = await db.user.create({ data })
  return user
}
```
'use server'는 Next.js 14 이후부터 추가된 서버 전용 함수(Server Action) 선언용 지시어이다. 파일이나 함수 맨 위에 "use server"를 적으면, Next.js는 그 함수를 서버 런타임에서만 실행되도록 강제한다.

서버에서 실행된다고 보안에 안전한 것은 아니고, 누구든 네트워크 요청을 재현할 수 있으니 인증/인가 작업은 필수이다.

## 예시
```ts
'use server'
import { db } from '@/lib/db' // Your database client
 
export async function fetchUsers() {
  const users = await db.user.findMany()
  return users
}
```
위 코드는 db에 접근하는 코드로, 오직 서버에서만 실행될 수 있고, 실행되어야 한다. 이 함수에 'use server' 지시어를 붙여 서버 측에서 수행되는 함수임을 명시해주었다.

```ts
'use client'
import { fetchUsers } from '../actions'
 
export default function MyButton() {
  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```
위 컴포넌트는 클라이언트 컴포넌트이며, `fetchUsers`를 호출한다. 이 경우 'use server'의 효과에 의해서 클릭 이벤트가 발생하는 경우 그 동작은 서버에서 수행된다. 이는 RPC로 통신하여 서버측의 함수를 호출시키기 때문이다. 이 경우 당연하지만 서버측에서 사용되는 코드는 클라이언트 측에 노출되지 않는다.

## 인라인 지시
```ts
import { EditPost } from './edit-post'
import { revalidatePath } from 'next/cache'
 
export default async function PostPage({ params }: { params: { id: string } }) {
  const post = await getPost(params.id)
 
  async function updatePost(formData: FormData) {
    'use server'
    await savePost(params.id, formData)
    revalidatePath(`/posts/${params.id}`)
  }
 
  return <EditPost updatePostAction={updatePost} post={post} />
}
```
'use server'를 함수 정의 내부 첫째줄에 기입하면, 해당 함수는 'use server'함수로 취급된다. 위 경우 updatePost는 호출될 때 오직 서버에서만 수행된다.

