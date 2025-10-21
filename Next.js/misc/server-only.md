---
상위 개념: "[[../Next.js|Next.js]]"
---
# server-only
server-only 라이브러리는 Next.js에서 동작하며, 서버에서만 실행되어야 할 파일의 상단에 선언하여 빌드타임에 이 코드가 클라이언트에서 사용되는 상황이 감지된 경우 빌드 에러를 뱉어준다.

```bash
Failed to compile.

./src/server/repository/ArticleRepository.ts
Error:   x You're importing a component that needs "server-only". That only works in a Server Component which is not supported in the pages/ directory. Read more: https://nextjs.org/docs/app/building-your-
  | application/rendering/server-components
  | 
```

## 설치 방법
```bash
npm i server-only
```