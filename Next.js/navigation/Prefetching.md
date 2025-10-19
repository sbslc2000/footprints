---
상위 개념: "[[Next.js]]"
---
# Prefetching

> Prefetching is the process of loading a route in the background before the user navigates to it.


Next.js에서 다른 페이지로 변경할 때, 서버에서 렌더링되어 내려오는 페이지는 두 가지 타입 중 하나이다.

1. Static Rendering(or Prerendering) : 빌드 타임에 미리 생성된 HTML을 내려받는다.
2. Dynamic Rendering : 사용자가 요청할 때 서버에서 HTML을 만들어 내려보낸다.

이 방식들은 사용자 요청 이후 HTML을 만들기 때문에(혹은 그저 제공하는데에만도) 지연 시간이 발생한다. Next.js는 이러한 문제를 해결하기 위해 사용자가 접근할 법한 페이지를 미리 받아온다. 이를 Prefetching이라고 한다. Prefetching을 통해 사용자는 미리 받아온 페이지로 즉시 이동하는 듯한 효과를 얻을 수 있다.

```ts
import Link from 'next/link'
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html>
      <body>
        <nav>
          {/* Prefetched when the link is hovered or enters the viewport */}
          <Link href="/blog">Blog</Link>
          {/* No prefetching */}
          <a href="/contact">Contact</a>
        </nav>
        {children}
      </body>
    </html>
  )
}
```

Next.js는 \<Link> 컴포넌트가 사용된 라우팅 링크에 대해서, 해당 링크가 사용자의 뷰포트에 드러설 때 해당 페이지를 미리 로드한다.

Prefetching이 동작하는 방식은 해당 링크가 정적 라우팅인지 동적 라우팅인지에 따라 달라진다. 정적 라우팅이라면 페이지 전체가 prefetch되며, 동적 라우팅이라면 (뷰포트에 들어왔을 때 어떤 페이지를 가져와야할 지 불분명할 수도 있으므로) loading.tsx만 prefetch한다. 