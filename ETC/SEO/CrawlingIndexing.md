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

<br />

## 3. XML Sitemaps

**사이트맵**은 Google과 소통하는 가장 쉬운 방법이다. Google이 새 콘텐츠를 쉽게 감지하고 웹사이트를 더 효율적으로 크롤링할 수 있도록 웹사이트에 속한 URL과 업데이트 시기를 나타낸다.

XML 사이트맵이 가장 많이 사용되기는 하지만 [RSS](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap)나 [Atom](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap)을 통해 만들 수도 있고, 단순성을 선호하는 경우 [Text](https://developers.google.com/search/docs/advanced/sitemaps/build-sitemap) 파일을 통해서도 만들 수 있다.

사이트맵은 사이트의 페이지, 비디오 및 기타 파일에 대한 정보와 이들 간의 관계를 제공하는 파일이다. Google과 같은 검색 엔진은 이 파일을 읽어 사이트를 보다 지능적으로 크롤링한다.

According to [Google](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview):

> 다음과 같은 경우 사이트맵이 필요할 수 있다:
>
> - **사이트의 규모가 정말 큰 경우**: Google 웹 크롤러는 새 페이지나 최근에 업데이트된 페이지 중 일부를 크롤링하는 것을 놓칠 가능성이 높다.
> - **사이트가 분리되고 잘 연결되지 않은 콘텐츠 페이지의 대용량 아카이브가 있는 경우**: 사이트 페이지가 자연스럽게 서로를 참조하지 않는 경우 사이트맵에 나열하여 Google이 일부 페이지를 놓치지 않도록 할 수 있다.
> - **새로운 사이트이면서 외부 링크가 거의 없는 경우**: Googlebot 및 기타 웹 크롤러는 한 페이지에서 다른 페이지로 연결되는 링크를 따라 웹을 탐색한다. 결과적으로 페이지에 연결된 다른 사이트가 없으면 Google에서 페이지를 찾지 못할 수 있다.
> - **사이트에 리치 미디어 콘텐츠(동영상, 이미지)가 많거나 Google 뉴스에 표시되는 경우**: Google은 적절한 검색을 위해 사이트맵의 추가 정보를 고려할 수 있다.

사이트맵은 훌륭한 검색 엔진 성능을 위한 필수 요소는 아니지만 봇의 크롤링 및 인덱싱을 용이하게 할 수 있다. 콘텐츠를 더 빨리 살펴보고 순위를 매길 수 있다.

사이트맵을 사용하는 경우 웹사이트 전체에 새 콘텐츠가 채워지면 사이트맵을 동적으로 만드는 것이 좋다. 정적 사이트맵도 유효하지만 지속적인 검색 목적으로 사용되지 않으므로 덜 유용할 수 있다.

### 3.1. Next.js 프로젝트에 사이트맵을 추가하는 방법

- #### **`Manual`**

  비교적 단순하고 정적인 사이트가 있는 경우 프로젝트의 공용 디렉터리에 `sitemap.xml`을 수동으로 만들 수 있다.

  ```xml
    <!-- public/sitemap.xml -->
    <xml version="1.0" encoding="UTF-8">
    <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      <url>
        <loc>http://www.example.com/foo</loc>
        <lastmod>2021-06-01</lastmod>
      </url>
    </urlset>
    </xml>
  ```

- #### **`getServerSideProps`**

  사이트가 동적일 가능성이 높다. 이 경우 `getServerSideProps`를 활용하여 주문형 XML 사이트맵을 만들 수 있다.

  `pages/sitemap.xml.js`와 같이 `pages` 디렉토리 내에 새 페이지를 만들 수 있다. 이 페이지의 목표는 API를 사용하여 동적 페이지의 URL로 알 수 있는 데이터를 얻는 것이다. 그런 다음 `/sitemap.xml`의 응답 XML 파일을 작성한다.

  예시:

```javascript
//pages/sitemap.xml.js
const EXTERNAL_DATA_URL = "https://jsonplaceholder.typicode.com/posts";

function generateSiteMap(posts) {
  return `<?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <!--이미 알고 있는 두 개의 URL을 수동으로 설정한다.-->
     <url>
       <loc>https://jsonplaceholder.typicode.com</loc>
     </url>
     <url>
       <loc>https://jsonplaceholder.typicode.com/guide</loc>
     </url>
     ${posts
       .map(({ id }) => {
         return `
       <url>
           <loc>${`${EXTERNAL_DATA_URL}/${id}`}</loc>
       </url>
     `;
       })
       .join("")}
   </urlset>
 `;
}

function SiteMap() {
  // getServerSideProps가 무거운 작업을 수행한다.
}

export async function getServerSideProps({ res }) {
  // 사이트의 URL을 수집하기 위해 API를 호출한다.
  const request = await fetch(EXTERNAL_DATA_URL);
  const posts = await request.json();

  // 게시물 데이터로 XML 사이트맵을 생성한다.
  const sitemap = generateSiteMap(posts);

  res.setHeader("Content-Type", "text/xml");
  // XML을 브라우저로 보낸다.
  res.write(sitemap);
  res.end();

  return {
    props: {},
  };
}

export default SiteMap;
```

> 추가 자료
>
> - Google: [Learn about Sitemaps](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview)
> - Google: [Overview of crawling and indexing topics](https://developers.google.com/search/docs/advanced/crawling/overview)

<br />

## 3. Special Meta Tags for Search Engines

**메타 로봇 ​​태그**는 검색 엔진이 항상 준수하는 지시어이다. 로봇 태그를 추가하면 웹 사이트의 색인 생성이 더 쉬워진다.

지시와 제안에는 차이가 있다. **메타 로봇 ​​태그** 또는 [`robots.txt`](https://nextjs.org/learn/seo/crawling-and-indexing/robots-txt) 파일은 **지시문**이며 항상 준수된다. **표준 태그**는 Google이 준수 여부를 결정할 수 있는 **권장 사항**이다.

페이지 수준 메타 태그의 많은 옵션이 있지만 다음은 일반적 SEO와 관련된 예시:

```html
<meta name="robots" content="noindex,nofollow" />
```

robots 태그는 아마 가장 많이 보게 될 태그일 것이다. 기본값은 `index,follow`로 따로 지정할 필요가 없으며 `all`은 유효한 대체 버전이다:

```html
<meta name="robots" content="all" />
```

위의 예에서와 같이 로봇 태그를 `noindex,nofollow`로 설정하면 검색 엔진에 다음과 같이 표시된다.

- **noindex**

  검색 결과에 이 페이지를 표시하지 않기 위해 사용한다. 생략하면 페이지를 인덱싱하여 검색 결과에 표시할 수 있음을 나타낸다.

  웹 사이트를 구축할 때 특정 페이지를 인덱싱하지 않을 수 있다. 설정 페이지, 내부 검색 페이지, 정책 페이지 등에 설정 할 수 있다.

- **nofollow**

  이 페이지의 링크를 따르지 않기 위해 사용한다. 생략하면 로봇이 이 페이지의 링크를 크롤링하고 따라갈 수 있다. 다른 페이지에서 찾은 링크는 크롤링을 사용하도록 설정할 수 있으므로 `링크 A`가 페이지 `X`와 `Y`에 나타나고 `X`에는 `nofollow` 로봇 태그가 있지만 `Y`에는 태그가 없으면 Google에서 링크를 크롤링하기로 설정할 수 있다.

> Google 공식 문서에서 [지시어의 전체 목록](https://developers.google.com/search/docs/advanced/robots/robots_meta_tag#directives)을 볼 수 있다.

### 3.1. Googlebot tag

```html
<meta name="googlebot" content="noindex,nofollow" />
```

때때로 `googlebot` 태그를 발견할 수 있다. 대부분의 경우 `robots`만 있지만 `googlebot` 태그는 Google에만 해당된다. Googlebot에 대한 별도의 규칙과 나머지 검색 봇에 대한 일반적인 규칙을 정할 경우 이 태그를 사용해라.

충돌하는 태그가 있는 경우 더 제한적인 태그를 적용한다.

크롤링을 원하지 않는 URL을 `robots.txt`에 추가할 수 있는 경우 이러한 태그가 필요한 이유가 궁금할 수 있다. 메타 태그는 요청 시 페이지를 `noindex`로 표시할 수 있는 유연성을 제공한다.

예를 들어 제품 페이지에 필터를 적용했는데 결과가 없는 경우 이 페이지는 `noindex`로 설정하는 것이 일반적이다.

robots.txt 파일을 통해 크롤링하는 봇으로부터 제한된 URL은 Google에서 절대 크롤링하지 않지만 페이지의 색인이 이미 생성된 후에 규칙이 추가되면 페이지의 색인이 생성된 상태로 유지될 수 있다. 페이지의 색인이 생성되지 않도록 하는 가장 좋은 방법은 `noindex` 태그를 사용하는 것다.

> Google은 페이지를 크롤링하지 않고 색인을 생성하도록 결정할 수 있다. 이것은 매우 드물지만 Google이 페이지가 특정 검색 결과를 이행하기를 원하고 페이지에 사용자가 기대하는 내용이 포함되어 있다고 확신할 때 발생한다.

### 3.2. Google tags

#### **nositelinkssearchbox**

```html
<meta name="google" content="nositelinkssearchbox" />
```

사용자가 사이트를 검색할 때 Google 검색 결과에 사이트에 대한 다른 직접 링크와 함께 사이트에 특정한 검색창이 표시되는 경우가 있다. 이 태그는 Google에 사이트링크 검색창을 표시하지 않도록 지시한다.

#### **notranslate**

```html
<meta name="google" content="notranslate" />
```

Google은 사이트 콘텐츠가 사용자가 읽고 싶어하는 언어로 되어 있지 않음을 인식하면 종종 검색 결과에 번역 링크를 제공한다.

일반적으로 이렇게 하면 훨씬 더 많은 사용자 그룹에 독특하고 매력적인 콘텐츠를 제공할 수 있다. 그러나 이것을 원하지 않을 수 있다. 이 메타 태그는 Google에서 이 페이지에 대한 번역을 제공하지 않기를 원한다고 Google에 알린다.

### 3.3. Example

접할 수 있는 몇 가지 일반적인 태그를 살펴보았으므로 다음은 그 중 일부를 사용하는 페이지의 예시이다:

```jsx
import Head from "next/head";

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Meta Tag Example</title>
        <meta name="google" content="nositelinkssearchbox" key="sitelinks" />
        <meta name="google" content="notranslate" key="notranslate" />
      </Head>
      <p>Here we show some meta tags off!</p>
    </div>
  );
}

export default IndexPage;
```

예제에서 볼 수 있듯이 페이지 `head`에 요소를 추가하기 위한 built-in component인 [next/head](https://nextjs.org/docs/api-reference/next/head)를 사용했다.

`head`에 태그가 중복되는 것을 방지하려면 태그가 한 번만 렌더링되도록 하는 `key` 속성을 사용할 수 있다.

<br />

## 4. What are Canonical Tags?

**표준** URL은 사이트의 중복된 페이지들 중 검색 엔진이 가장 대표적이라고 생각하는 페이지의 URL이다.

표준 URL을 검색 엔진에 직접 전달할 수 있지만 그러지 않고 여러 URL을 그룹화할 수도 있다. Google이 여러 경로에서 URL을 찾을 수 있는 경우 자동으로 그룹화할 수 있다.

구글은 이러한 것들을 탐지하는 데는 훌륭하지만, 구글의 시스템은 대규모로 작동하며 모든 경우를 다루지는 않는다. 우수한 성능을 보장하기 위해서 표준 태그는 중요하다.

Google은 동일한 내용을 가진 여러 개의 URL을 찾을 경우 중복된 것으로 간주될 수 있으므로 검색 결과에서 해당 URL을 강등할 수 있다.

이는 도메인 간에도 발생한다. 두 개의 다른 웹 사이트를 실행하고 각 웹 사이트에 동일한 내용을 게시하면 검색 엔진은 둘 중 하나를 선택하여 순위를 매길지 둘 다 강등할지 결정할 수 있다.

여기서 표준 태그가 매우 유용하다. 그들은 구글에게 어떤 URL이 원본이고 어떤 URL이 복제됐는지 알려준다. 동일하거나 다른 도메인에 중복된 페이지가 많으면 순위가 떨어지거나 불이익을 받을 수 있다.

e-commerce에서 `example.com/products/phone` 및 `example.com/phone`을 통해 제품에 접근할 수 있다고 가정해 보자.

둘 다 유효하고 동작하는 URL이지만, 중복 컨텐츠 감지를 방지하기 위해 표준 태그를 사용한다. 순위에 `https://example.com/products/phone`을 고려하려면 표준 태그를 설정한다.

```html
<link rel="canonical" href="https://example.com/products/phone" />
```

제작자, 사용자, 마케팅 도구는 다양한 URL을 만들 수 있기 때문에 표준 태그는 SEO에서 기본이다.

Google에서 일부 마케팅을 진행하고 있다고 가정하면 Google은 [UTM 매개 변수](https://ga-dev-tools.web.app/campaign-url-builder/)를 추가한다. 이 새로운 고유 URL은 Googlebot에 의해 인덱싱될 수 있으므로 중복된 페이지를 통합하기 위해 표준 태그를 계속 표시해야 한다.

#### **Example**

```jsx
import Head from "next/head";

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Canonical Tag Example</title>
        <link
          rel="canonical"
          href="https://example.com/blog/original-post"
          key="canonical"
        />
      </Head>
      <p>This post exists on two URLs.</p>
    </div>
  );
}

export default IndexPage;
```

> **추가 자료**
>
> - Google: [Consolidate Duplicate URLs](https://developers.google.com/search/docs/advanced/crawling/consolidate-duplicate-urls)
> - Next.js: [i18n Routing](https://nextjs.org/docs/advanced-features/i18n-routing)
