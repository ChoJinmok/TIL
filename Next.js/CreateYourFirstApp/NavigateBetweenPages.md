# Navigate Between Pages

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Navigate Between Pages](https://nextjs.org/learn/basics/navigate-between-pages)내용들을 정리했습니다.

<br />

## 1. Pages in Next.js

- Next.js에서 페이지는 [`pages` 디렉토리](https://nextjs.org/docs/basic-features/pages)의 파일에서 export한 React Component이다.

- 페이지는 파일 이름을 기반으로 경로와 연결된다. 예를 들어:

  - `pages/index.js`는 `/` 경로와 연결되어 있다.
  - `pages/posts/first-post.js`는 `/posts/first-post` 경로와 연결되어 있다.

- Component는 어떤 이름이든 지정할 수 있지만 `export default`로 내보내야 한다.

- [`pages` 디렉토리](https://nextjs.org/docs/basic-features/pages) 하단에 JS 파일을 생성하면 파일의 경로가 URL 경로가 된다.

- 어떻게 보면 HTML이나 PHP 파일을 사용하여 웹사이트를 구축하는 것과 유사하지만 HTML을 작성하는 대신 JSX를 작성하고 React Components를 사용한다.

<br />

## 2. Link Component

웹사이트에서 페이지를 연결할 때 `<a>` HTML 태그를 사용한다.

Next.js에서 [`next/link`](https://nextjs.org/docs/api-reference/next/link)의 Link Component를 사용하여 애플리케이션의 페이지 사이를 연결할 수 있다. `<Link>`를 사용하면 클라이언트 측 탐색을 수행할 수 있으며 탐색 동작을 더 잘 제어할 수 있는 [props](https://nextjs.org/docs/api-reference/next/link)을 사용할 수 있다.

### 2.1. Using `<Link>`

맨 위에 다음 줄을 추가하여 `next/link`에서 `Link` component를 import 한다.

```typescript
import Link from "next/link";

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">Back to home</Link>
      </h2>
    </>
  );
}
```

> Next.js 12.2 이전에는 Link component가 `<a>` 태그를 래핑해야 했지만 버전 12.2 이상에서는 필요하지 않다.

<br />

## 3. Client-Side Navigation

[Link](https://nextjs.org/docs/api-reference/next/link) component를 사용하면 동일한 Next.js 앱의 두 페이지 간 클라이언트 측 탐색이 가능하다.

클라이언트 측 탐색을 한다는 것은 브라우저에서 수행되는 기본 탐색이 아닌 JavaScript를 사용하여 빠른 페이지 전환이 일어난다는 것을 의미한다.

확인할 수 있는 간단한 방법:

- 브라우저의 개발자 도구를 사용하여 `<html>`의 배경 CSS 속성을 노란색으로 변경한다.
- 링크를 클릭해서 두 페이지 사이를 앞뒤로 이동한다.
- 페이지 전환 시에 노란색 배경이 지속되는 것을 볼 수 있다.

이는 브라우저가 전체 페이지를 로드하지 않고 클라이언트측 탐색이 작동하고 있음을 나타낸다.

![Client-Side Navigation](https://nextjs.org/static/images/learn/navigate-between-pages/client-side.gif)

`<Link href="…">` 대신 `<a href="…">`를 사용하면 브라우저가 전체가 새로고침되기 때문에 링크 클릭 시 배경색이 지워진다.

### 3.1. Code splitting and prefetching

Next.js는 자동으로 코드 분할을 수행하므로 각 페이지는 해당 페이지에 필요한 항목만 로드한다. 즉, 홈페이지가 렌더링될 때 다른 페이지에 대한 코드는 처음에 제공되지 않는다.

이렇게 하면 수백 페이지가 있는 경우에도 홈페이지가 빠르게 로드된다.

요청한 페이지에 대한 코드만 로드한다는 건 페이지가 격리되는 것을 의미한다. 특정 페이지에서 오류가 발생해도 애플리케이션의 나머지 부분은 계속 작동한다.

또한 Next.js의 프로덕션 빌드에서 [`Link`](https://nextjs.org/docs/api-reference/next/link) component가 브라우저의 뷰포트에 나타날 때마다 Next.js는 백그라운드에서 연결된 페이지의 코드를 자동으로 **미리 가져온다**. 링크를 클릭하면 대상 페이지의 코드가 이미 백그라운드에 로드되고 페이지 전환이 거의 즉각적으로 이루어진다.

### 3.2. Summary

Next.js는 코드 분할, 클라이언트측 탐색 및 프리페치(프로덕션에서)를 통해 애플리케이션을 자동으로 최적화하여 최상의 성능을 발휘한다.

[`pages`](https://nextjs.org/docs/basic-features/pages) 하단의 파일로 경로를 만들고 내장된 [Link](https://nextjs.org/docs/api-reference/next/link) component를 사용한다. 라우팅 라이브러리가 필요하지 않다.

[라우팅 설명서](https://nextjs.org/docs/routing/introduction)의 일반적인 라우팅 및 [`next/link`의 API 참조](https://nextjs.org/docs/api-reference/next/link)에서 `Link` component에 대해 자세히 알아볼 수 있다.

> Next.js 앱 외부의 외부 페이지에 연결해야 하는 경우 `Link` 없이 `<a>` 태그만 사용하면 된다.
