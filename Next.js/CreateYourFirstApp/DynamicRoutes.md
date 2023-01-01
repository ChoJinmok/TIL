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

<br />

## 2. Implement getStaticPaths

파일 설정:

- `pages/posts` 디렉터리 내에 `[id].js`라는 파일을 만든다.
- 또한 `pages/posts` 디렉토리에서 `first-post.js`를 제거 — 더 이상 사용하지 않는다.

그런 다음 편집기에서 `pages/posts/[id].js`를 열고 다음 코드를 붙여넣는다. `...`는 나중에 채울거다.

```jsx
import Layout from "../../components/layout";

export default function Post() {
  return <Layout>...</Layout>;
}
```

그런 다음 `lib/posts.js`를 열고 하단에 다음 `getAllPostIds` 함수를 추가한다. `posts` 디렉토리에 있는 파일 이름 목록(`.md` 제외)을 반환한ㄴ다.

```jsx
export function getAllPostIds() {
  const fileNames = fs.readdirSync(postsDirectory);

  // 다음과 같은 배열을 return 한다:
  // [
  //   {
  //     params: {
  //       id: 'ssg-ssr'
  //     }
  //   },
  //   {
  //     params: {
  //       id: 'pre-rendering'
  //     }
  //   }
  // ]
  return fileNames.map((fileName) => {
    return {
      params: {
        id: fileName.replace(/\.md$/, ""),
      },
    };
  });
}
```

**important**: 반환된 목록은 단순한 문자열 배열이 아니라 위의 주석처럼 보이는 개체 배열이어야 한다. 각 객체는 `params` 키를 가지고 있어야 하며 `id` 키가 있는 객체를 포함해야 한다(파일 이름에 `[id]`를 사용하기 때문). 그렇지 않으면 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)가 실패한다.

마지막으로 `getAllPostIds` 함수를 가져와 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation) 내에서 사용한다. `pages/posts/[id].js`를 열고 내보낸 `Post` component 위에 다음 코드를 복사한다.

```jsx
import { getAllPostIds } from "../../lib/posts";

export async function getStaticPaths() {
  const paths = getAllPostIds();
  return {
    paths,
    fallback: false,
  };
}
```

- `paths`는 `pages/posts/[id].js`가 정의한 매개변수를 포함하고 `getAllPostIds()`가 반환한 경로 배열이다. [`paths` key 문서](https://nextjs.org/docs/basic-features/data-fetching/overview#the-paths-key-required)에서 자세히 알아보기.
- 지금은 [`fallback: false`](https://nextjs.org/docs/basic-features/data-fetching#fallback-false) 무시하고 넘어가기.

<br />

## 3. Implement getStaticProps

주어진 `id`로 게시물을 렌더링하기 위해 필요한 데이터를 가져와야 한다.

이렇게 하려면 `lib/posts.js`의 가장 아래에 `getPostData` 함수를 추가한다. `id`를 기반으로 게시물 데이터를 반환한다.

```jsx
export function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`);
  const fileContents = fs.readFileSync(fullPath, "utf8");

  // gray-matter을 사용하여 게시물 메타데이터 섹션을 구문 분석한다.
  const matterResult = matter(fileContents);

  // 데이터를 id와 결합
  return {
    id,
    ...matterResult.data,
  };
}
```

그런 다음 `pages/posts/[id].js`를 열고 다음 줄을 바꿔준다.

```jsx
import { getAllPostIds, getPostData } from "../../lib/posts";

export async function getStaticProps({ params }) {
  const postData = getPostData(params.id);
  return {
    props: {
      postData,
    },
  };
}
```

게시물 페이지는 이제 `getStaticProps`의 `getPostData` 함수를 사용하여 게시물 데이터를 가져오고 이를 props로 반환한다.

이제 `postData`를 사용하도록 `Post` component를 업데이트한다. `pages/posts/[id].js`에서 export 한 `Post` component를 다음 코드로 바꾼다.

```jsx
export default function Post({ postData }) {
  return (
    <Layout>
      {postData.title}
      <br />
      {postData.id}
      <br />
      {postData.date}
    </Layout>
  );
}
```

### 3.1. Something Wrong?

오류가 발생하면 코드를 확인:

- `pages/posts/[id].js`는 [다음](https://github.com/vercel/next-learn/blob/master/basics/dynamic-routes-step-1/pages/posts/%5Bid%5D.js)과 같아야 한다.
- `lib/posts.js`는 [다음](https://github.com/vercel/next-learn/blob/master/basics/dynamic-routes-step-1/lib/posts.js)과 같아야 한다.
- (여전히 작동하지 않는 경우) 나머지 코드는 [다음](https://github.com/vercel/next-learn/tree/master/basics/dynamic-routes-step-1)과 같아야 한다.

여전히 문제가 있는 경우 [GitHub Discussions](https://github.com/vercel/next.js/discussions)에서 커뮤니티에 자유롭게 질문해라. 다른 사람들이 볼 수 있도록 코드를 GitHub에 푸시하고 링크하면 된다.

### 3.2. 요약

![how-to-dynamic-routes (1)](<./images/how-to-dynamic-routes%20(1).png>)
