# API Routes

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [API Routes](https://nextjs.org/learn/basics/api-routes)내용들을 정리했습니다.

<br />

Next.js는 [API Routes](https://nextjs.org/docs/api-routes/introduction)를 지원하는데 Node.js serverless function으로 API 엔드포인트를 쉽게 생성할 수 있다.

## 1. Creating API Routes

[API Routes](https://nextjs.org/docs/api-routes/introduction)를 사용하면 Next.js 앱 내부에 API 엔드포인트를 생성할 수 있다. `pages/api` 디렉토리 내에 함수를 생성해서 이를 구현한다:

```javascript
// req = HTTP incoming message, res = HTTP server response
export default function handler(req, res) {
  // ...
}
```

> [API Routes 문서](https://nextjs.org/docs/api-routes/introduction)에서 위의 요청 핸들러에 대해 자세히 알아보기.

서버리스 함수(람다라고도 한다)로 배포할 수 있다.

### 1.1. Creating a simple API endpoint

`pages/api`에 `hello.js`라는 파일을 만든다.

```javascript
export default function handler(req, res) {
  res.status(200).json({ text: "Hello" });
}
```

http://localhost:3000/api/hello에서 액세스하면 `{"text":"Hello"}`가 표시된다:

- `req`는 [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)의 인스턴스이면서 미리 빌드된 [미들웨어](https://nextjs.org/docs/api-routes/request-helpers)이다.
- `res`는 [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)의 인스턴스이면서 [helper function](https://nextjs.org/docs/api-routes/response-helpers)이다.

<br />

## 2. API Routes Details

다음은 [API Routes](https://nextjs.org/docs/api-routes/introduction)에 대해 알아야 할 것들이다.

### 2.1. `getStaticProps` 또는 `getStaticPaths`에서 API Route를 가져오지 않는다.

[`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation) 또는 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)에서 API Route를 가져오면 안 된다. 대신 서버측 코드를 [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation) 또는 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)에 직접 작성하거나 helper function을 호출해라.

이유는 다음과 같다. [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticprops-static-generation) 및 [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching/overview#getstaticpaths-static-generation)는 서버 측에서만 실행되며 클라이언트 측에서는 실행되지 않는다. 또한 이러한 함수들은 브라우저용 JS 번들에 포함되지 않는다. 즉, 직접 데이터베이스 쿼리와 같은 코드를 브라우저로 보내지 않고도 작성할 수 있다. 자세한 내용은 [Writing Server-Side code](https://nextjs.org/docs/basic-features/data-fetching/get-static-props#write-server-side-code-directly) 문서를 참조.

### 2.2. 좋은 사용 사례: Form Input 처리

API Routes의 좋은 사용 사례는 form input을 처리하는 것이다. 예를 들어 페이지에서 form을 만들고 API Route에 `POST` 요청을 보내도록 할 수 있다. 그런 다음 코드를 작성하여 데이터베이스에 직접 저장할 수 있다. API Route 코드는 클라이언트 번들의 일부가 아니므로 안전하게 서버 측 코드를 작성할 수 있다.

```jsx
export default function handler(req, res) {
  const email = req.body.email;
  // 그런 다음 이메일을 데이터베이스 등에 저장, etc...
}
```

### 2.3. Preview Mode

[Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended)은 페이지가 헤드리스 CMS에서 데이터를 가져올 때 유용하다. 그러나 헤드리스 CMS에서 초안을 작성할 때 페이지에서 즉시 초안을 **미리 보고** 싶을 때는 적합하지 않다. Next.js가 빌드를 할 때가 아니라 **요청을 할 때**에 이러한 페이지를 렌더링하고 게시된 콘텐츠 대신 초안 콘텐츠를 가져오길 원할 것이다. 이 특정한 경우에만 Next.js가 Static Generation을 우회하기를 원할 것이다.

Next.js는 위의 문제를 해결하기 위해 **미리보기 모드**라는 기능이 있으며 [API Routes](https://nextjs.org/docs/api-routes/introduction)를 활용한다. 자세한 내용은 [미리보기 모드](https://nextjs.org/docs/advanced-features/preview-mode) 문서를 참조.

### 2.4. Dynamic API Routes

API Route는 일반 페이지와 마찬가지로 동적일 수도 있다. 자세한 내용은 [Dynamic API Routes](https://nextjs.org/docs/api-routes/dynamic-api-routes) 문서를 참조.
