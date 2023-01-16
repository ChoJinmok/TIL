# Crawling and Indexing

> Next.js 튜토리얼 SEARCH ENGINE OPTIMIZATION 중 [Crawling and Indexing](https://nextjs.org/learn/seo/crawling-and-indexing)내용들을 정리했습니다.

<br />

검색 시스템과 Googlebot의 작동 방식에 대한 일반적인 개요를 살펴봤다. 크롤링 및 인덱싱에 영향을 미치는 요소에 대해 알아보겠다.

## 1. What are HTTP Status Codes?

[HTTP 응답 상태 코드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)는 HTTP 요청 성공 여부를 나타낸다. 많은 상태 코드가 있지만 SEO 컨텍스트에서 의미 있는 것은 소수에 불과하다.

### 1.1. 200

> [`HTTP 200 OK`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) 성공 상태 응답 코드는 요청이 성공했음을 나타낸다.

Google에서 페이지의 색인을 생성하려면 상태 코드가 `200`이여야 한다. 이는 유기적 트래픽을 수신하기 위해 페이지를 찾는 것이다.

### 1.2. 301/308

> [`HTTP 301 Moved Permanently`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301) Redirect 상태 응답 코드는 요청된 리소스가 최종적으로 대상 URL로 이동되었음을 나타낸다.

이것은 영구 리디렉션이다. 일반적으로 가장 널리 사용되는 리디렉션 유형이다.

URL이 A지점에서 B지점으로 이동되었을 때 리디렉션을 사용한다.

콘텐츠가 한 위치에서 다른 위치로 이동하는 경우 현재 및 잠재 고객을 잃지 않고 봇이 사이트를 계속 색인화하도록 하려면 리디렉션이 필요하다.

> [Next.js 영구 리디렉션](https://nextjs.org/docs/api-reference/next.config.js/redirects)은 최신 버전이면서 더 나은 옵션으로 간주되는 301 대신 기본적으로 308을 사용한다.

두 상태 코드의 주요 차이점은 `301`은 요청 방법을 `POST`에서 `GET`으로 변경할 수 있지만 `308`은 그렇지 않다는 것이다.

`getStaticProps()` 함수에서 props 대신 리디렉션을 반환하여 Next.js에서 `308` 리디렉션을 트리거할 수 있다.

```jsx
//  pages/about.js
export async function getStaticProps(context) {
  return {
    redirect: {
      destination: "/",
      permanent: true, // triggers 308
    },
  };
}
```

`next.config.js`에 설정된 리디렉션에서 `permanent: true` 키를 사용할 수도 있다.

```javascript
//next.config.js

module.exports = {
  async redirects() {
    return [
      {
        source: "/about",
        destination: "/",
        permanent: true, // triggers 308
      },
    ];
  },
};
```

### 1.3. 302

> [`HTTP 302 Found`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302) 리디렉션 상태 응답 코드는 요청된 리소스가 일시적으로 대상 URL로 이동되었음을 나타낸다.

대부분의 경우 `302` 리디렉션은 `301` 리디렉션이어야 한다. 일시적으로 사용자를 특정 페이지(예: 프로모션 페이지)로 리디렉션하거나 위치를 기반으로 사용자를 리디렉션하는 경우에는 그렇지 않을 수 있다.

### 1.4. 404

> [`HTTP 404 Not Found`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404) 클라이언트 오류 응답 코드는 서버가 요청된 리소스를 찾을 수 없음을 나타낸다.

앞서 언급한 것처럼 이동된 페이지는 `HTTP 301` 상태 코드와 함께 새 위치로 리디렉션되어야 한다. 그렇지 않으면 URL이 `404` 상태 코드를 반환할 수 있다.

`404` 상태 코드는 사용자가 존재하지 않는 URL을 방문하는 경우 원하는 결과이므로 반드시 나쁜 것은 아니지만 검색 순위가 떨어질 수 있으므로 페이지 내에서 빈번하게 발생 되어서는 안된다.

Next.js는 애플리케이션에 존재하지 않는 URL에 대해 자동으로 `404` 상태 코드를 반환한다.

props 대신 다음과 같이 return하면 경우에 따라 페이지에서 `404` 상태 코드를 반환할 수 있다.

```jsx
export async function getStaticProps(context) {
  return {
    notFound: true, // triggers 404
  };
}
```

`pages/404.js`를 생성하면 빌드 시 정적으로 [`사용자 지정 404 페이지를 생성`](https://nextjs.org/docs/advanced-features/custom-error-page#404-page)할 수 있다.

```javascript
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}
```

### 1.5. 410

> [`HTTP 410 Gone`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/410) 클라이언트 오류 응답 코드는 원본 서버에서 대상 리소스에 대한 액세스를 더 이상 사용할 수 없으며 이 상태가 영구적일 가능성이 있음을 나타낸다.

자주 사용되지는 않지만 웹사이트에서 더 이상 존재하지 않을 콘텐츠를 삭제하려는 경우 이 상태 코드를 찾아볼 수 있다.

`HTTP 410 Gone` 사용 예시:

- **E-Commerce**: 재고에서 영구적으로 제거된 제품
- **Forum**: 사용자가 삭제한 스레드
- **Blog**: 사이트에서 제거된 블로그 게시물

이 상태 코드는 봇이 이 콘텐츠를 크롤링할 필요 없다고 알려준다.

### 1.6. 500

> [`HTTP 500 Internal Server Error`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) 응답 코드는 서버가 요청을 이행하지 못하게 하는 예기치 않은 조건이 발생했음을 나타낸다.

Next.js는 예기치 않은 애플리케이션 오류에 대해 자동으로 `500` 상태 코드를 반환한다. `pages/500.js`를 생성하여 빌드 시 정적으로 [사용자 지정 `500` 오류 페이지를 생성](https://nextjs.org/docs/advanced-features/custom-error-page#500-page)할 수 있다.

```jsx
// pages/500.js
export default function Custom500() {
  return <h1>500 - Server-side error occurred</h1>;
}
```

### 1.7. 503

> [`HTTP 503 Service Unavailable`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503) 서버 오류 응답 코드는 서버가 요청을 처리할 준비가 되지 않았음을 나타낸다.

웹 사이트가 다운되고 웹 사이트가 장기간 다운될 것으로 예상되는 경우 이 상태 코드를 반환하는 것이 좋다. 이것은 장기적으로 순위가 떨어지는 것을 방지한다.

<br />

## 2. What is a robots.txt File?

[robots.txt](https://developers.google.com/search/docs/crawling-indexing/robots/intro) 파일은 페이지나 파일을 검색 엔진 크롤러에게 알린다. `robots.txt` 파일은 [좋은 봇](https://www.cloudflare.com/ko-kr/learning/bots/how-to-manage-good-bots/)이 특정 도메인에서 무언가를 요청하기 전 소비하는 웹 표준 파일이다.

CMS, admin, e-commerce 사용자 계정, 일부 API routes와 같은 웹 사이트는 크롤링되어 색인이 생성되지 않도록 특정 영역을 보호할 수 있다.

이러한 파일은 각 호스트의 루트에서 제공되어야 한다. 그렇지 않으면 루트 `/robots.txt` 경로를 대상 URL로 리디렉션할 수 있으며 대부분의 봇이 이를 따른다.

### 2.1. Next.js 프로젝트에 robots.txt 파일을 추가하는 방법

Next.js의 [정적 파일 제공](https://nextjs.org/docs/basic-features/static-file-serving) 덕분에 `robots.txt` 파일을 쉽게 추가할 수 있다. 루트 디렉터리의 `public` 폴더에 `robots.txt`라는 새 파일을 만든다.

이 파일에 넣을 수 있는 항목의 예:

```txt
//robots.txt

# Block all crawlers for /accounts
User-agent: *
Disallow: /accounts

# Allow all crawlers
User-agent: *
Allow: /
```

`yarn dev`로 앱을 실행하면 http://localhost:3000/robots.txt에서 사용할 수 있다. `public` 폴더 이름은 URL의 일부가 아니다.

`public` 디렉토리에 다른 이름을 지정하면 안된다. static assets을 제공하는 데 사용되는 유일한 디렉토리이다.

> 추가 자료
>
> - Google: [Create and Submit a `robots.txt` File](https://developers.google.com/search/docs/crawling-indexing/robots/create-robots-txt)
