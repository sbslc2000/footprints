---
상위 개념: "[[Next.js Navigation]]"
---
# Streaming
스트리밍은 동적 라우팅에서 서버가 미리 제공할 수 있는 렌더링 결과물을 먼저 제공하고, 이후에 동적 렌더링을 수행한 뒤 그 결과물을 받게 하여 사용자가 빠르게 라우팅 결과를 확인하게 하는 방법이다.

![](https://i.imgur.com/gVkSWJu.png)

스트리밍을 사용하기 위해서 loading.tsx를 라우팅 폴더에 추가해줄 수 있다.

```ts
export default function Loading() {
  // Add fallback UI that will be shown while the route is loading.
  return <LoadingSkeleton />
}
```
위와 같은 경우, Link로 연결된 페이지는 동적 페이지가 실제로 만들어지기 전에 (사용자가 요청하기 전에) loading.tsx는 prefetch된다. 따라서 사용자는 클릭하는 즉시 스켈레톤을 볼 수 있게 된다. 이후 동적 페이지가 로딩된 후에는 기존 콘텐츠는 swap된다.