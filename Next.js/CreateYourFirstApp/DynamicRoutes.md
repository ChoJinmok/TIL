# Dynamic Routes

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Dynamic Routes](https://nextjs.org/learn/basics/dynamic-routes)내용들을 정리했습니다.

<br />

우리는 페이지의 URL이 데이터에 의존하기를 원할 수 있다. 이를 위해선 [동적 경로](https://nextjs.org/docs/routing/dynamic-routes)를 사용해야 한다.

### 이 단원에서 배울 내용

- [동적 경로](https://nextjs.org/docs/routing/dynamic-routes)에서 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)를 사용해서 페이지를 정적으로 생성하는 방법
- 각 블로그 게시물에 대한 데이터를 가져오기 위해 [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation)를 작성하는 방법.
- [`remark`](https://github.com/remarkjs/remark)를 사용하여 마크다운을 렌더링하는 방법.
- 날짜 문자열을 예쁘게 인쇄하는 방법.
- [동적 경로](https://nextjs.org/docs/routing/dynamic-routes)가 있는 페이지에 연결하는 방법.
- [동적 경로](https://nextjs.org/docs/routing/dynamic-routes)에 대한 몇 가지 유용한 정보

<br />

## 1. 외부 데이터에 따른 페이지 경로

**페이지 콘텐츠**가 외부 데이터에 의존하는 경우를 다루어 봤다. [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation)를 사용하여 인덱스 페이지를 렌더링하는 데 필요한 데이터를 가져왔었다.

이번엔 각 **페이지 경로**가 외부 데이터에 의존하는 경우에 대해 이야기해보겠다. Next.js를 사용하면 외부 데이터에 의존하는 경로로 페이지를 정적으로 생성할 수 있다. 이렇게 하면 Next.js에서 `동적 URL`이 활성화된다.

![page-path-external-data](./images/page-path-external-data.png)

### 1.1. 동적 경로로 페이지를 정적으로 생성하는 방법

블로그 게시물에 대한 [`동적 경로`](https://nextjs.org/docs/routing/dynamic-routes)를 만들고 싶은 경우:

- 각 게시물의 경로는 `/posts/<id>`이며, 여기서 `<id>`는 `posts` 디렉터리 아래의 마크다운 파일 이름이다.
- `ssg-ssr.md` 및 `pre-rendering.md`가 있으므로 경로를 `/posts/ssg-ssr` 및 `/posts/pre-rendering`으로 지정하고 싶은 경우이다.

### 1.2. Overview of the Steps

먼저 `pages/posts` 아래에 `[id].js`라는 페이지를 만든다. `[`로 시작하고 `]`로 끝나는 페이지는 Next.js의 [동적 경로](https://nextjs.org/docs/routing/dynamic-routes)이다.

`pages/posts/[id].js`에서 다른 페이지와 마찬가지로 게시물 페이지를 렌더링하는 코드를 작성한다.

```jsx
import Layout from "../../components/layout";

export default function Post() {
  return <Layout>...</Layout>;
}
```

이 페이지에서 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)라는 비동기 함수를 export 할거다. 이 함수에서는 `id`에 `가능한 값` 목록을 반환해야 한다.

```jsx
import Layout from "../../components/layout";

export default function Post() {
  return <Layout>...</Layout>;
}

export async function getStaticPaths() {
  // id에 가능한 값 목록을 return
}
```

마지막으로 [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation)를 다시 구현해야 한다. 이번에는 주어진 `id`로 블로그 게시물에 필요한 데이터를 가져온다. [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation)에는 `id`를 포함하는 `params`가 제공된다(파일 이름이 `[id].js`이기 때문에).

```jsx
import Layout from "../../components/layout";

export default function Post() {
  return <Layout>...</Layout>;
}

export async function getStaticPaths() {
  // id에 가능한 값 목록을 return
}

export async function getStaticProps({ params }) {
  // params.id를 사용하여 블로그 게시물에 필요한 데이터를 가져온다.
}
```

![how-to-dynamic-routes](./images/how-to-dynamic-routes.png)
