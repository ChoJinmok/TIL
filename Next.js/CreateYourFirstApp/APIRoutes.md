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
