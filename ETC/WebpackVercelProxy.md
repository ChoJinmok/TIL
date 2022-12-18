# Webpack(dev),Vercel(production)에서 proxy로 COR 해결

> Next.js에서 API Routes를 이용해서 serverless api를 구현했는데 이를 다를 프로젝트에서 사용할 수 있을까하는 궁금점에서 해결책을 찾아봤다.

<br />

## 1. 역시 CORS error 발생

- Next.js로 진행한 프로젝트를 Vercel에 배포해놓은 상태였는데 API Routes 코드는 결국 Vercel 서버에서 구동된다.

- 그런데 **동일 도메인 + 동일 포트**가 아니기 때문에 CORS 에러가 발생했다.

## 2. 결국 해답은 Proxy server

- 서버에서 White List를 설정하거나 CORS에러를 해결할 수 없는 상황이었다.

- 그래서 결국 Proxy 서버를 활용할 방법을 찾아봤다.

## 3. Proxy server 구현

### 3.1. Webpack devServer proxy

- 개발환경은 Webpack으로 진행하고 있었기 때문에 Webpack에서 proxy를 구현할 방법이 있는지 찾아봤는데 Webpack의 devServer proxy가 있었다.

- webpack 설정 파일에 proxy 추가

```javascript
 // webpack 서버 설정
devServer: {

    ...

    proxy: {
        '/api': { // /api/로 시작하는 url은 아래의 전체 도메인을 추가하고, 옵션을 적용
            target: 'https://nextjsintro-flame.vercel.app', // 클라이언트에서 api로 보내는 요청은 해당 주소5로 바꿔서 보내겠다 라는 뜻
            changeOrigin: true, // cross origin 허용 설정
        },
    }
},
```

### 3.2. Vercel rewrites

- 서버 환경은 Vercel로 배포를 했기 때문에 Vercel의 rewrites기능을 활용해서 Proxy를 구현했다.

```json
{
  "rewrites": [
    // SPA Fallback
    {
      "source": "/((?!api/.*).*)", // 주소에 api가 있는 경우는 제외
      "destination": "/index.html"
    },
    // Proxy
    {
      "source": "/api/:match*",
      "destination": "https://nextjsintro-flame.vercel.app/api/:match*"
    }
  ]
}
```

## 4. 실제 코드

```jsx
export default function HomePage() {
  useEffect(() => {
    // 다음과 같이 요청을 보내면 https://nextjsintro-flame.vercel.app/api/hello로 요청이 간다.
    fetch("/api/hello")
      .then((res) => res.json())
      .then((res) => console.log(res));
  }, []);

  // ...
}
```

## 5. Reference

[[React] Webpack Proxy설정을 이용하여 CORS 해결](https://jforj.tistory.com/149)
[[webpack] proxy를 사용하여 CORS 에러 해결 하기!](https://velog.io/@jjhstoday/webpack-proxy%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-CORS-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0-%ED%95%98%EA%B8%B0)
[Vercel Rewrite: SPA Fallback except for API endpoint](https://stackoverflow.com/questions/64920230/vercel-rewrite-spa-fallback-except-for-api-endpoint)
