# Introduction to SEO

> Next.js 튜토리얼 SEARCH ENGINE OPTIMIZATION 중 [Introduction to SEO](https://nextjs.org/learn/seo/introduction-to-seo)내용들을 정리했습니다.

<br />

## 1. What is SEO?

SEO는 **Search Engine Optimization**(검색 엔진 최적화)를 의미한다. SEO의 목표는 검색 엔진 결과에서 순위를 높이는 것이다. 순위가 높을수록 사이트에 더 많은 트래픽이 발생하여 더 많은 비즈니스로 이어진다!

<br />

## 2. Why is SEO so important?

SEO는 브랜드에 대한 전환율과 신뢰도를 높이는 열쇠이다. 더 높은 검색 순위 배치는 더 많은 유기적 방문자로 어이진다. **검색 엔진 유기적 트래픽**(검색 엔진에서 결과를 클릭하여 귀하의 사이트를 방문하는 방문자)은 세 가지 이유로 많은 비즈니스에서 중요하다.

1. **Qualitative** – 방문자가 고객으로 전환될 가능성이 높아진다.
2. **Trustable** – 브랜드에 대한 높은 신뢰도.
3. **Low-Cost** – 검색 엔진 순위를 높이기위해선 SEO에 시간과 노력만 투자하면된다. 상위 유기적 검색 결과 위치에 표시하는데 드는 직접적인 비용이 없다.

> [검색 엔진 마케팅(SEM)](https://learndigital.withgoogle.com/digitalgarage/course/promote-business-online/lesson/54)는 검색 결과 상단의 콘텐츠가 100% 유료이고 Sponsored 레이블을 가진다. 유기적 결과로 만들어지는 검색 엔진 최적화와는 다르다.

### 2.1. 최적화의 세 가지 요소

웹 사이트 최적화 프로세스:

1. **Technical** – 크롤링 및 웹 성능을 위해 웹사이트를 최적화한다.
2. **Creation** – 특정 키워드를 타겟팅하는 콘텐츠 전략을 만든다.
3. **Popularity** – 검색 엔진이 신뢰할 수 있는 소스임을 알 수 있도록 사이트의 인지도를 높인다. 사이트로 다시 연결되는 타사 사이트인 [백링크](https://moz.com/learn/seo/backlinks)로 인지도를 높일 수 있다.

SEO 분야는 광범위하고 [고려해야할 것](https://learningseo.io/)들이 많지만 몇 모범 사례를 통해 웹 앱을 SEO에 대비할 수 있는 방법을 이해할 수 있다.

<br />

## 3. Search Systems

검색 시스템은 일반적으로 검색 엔진(Google, Bing, DuckDuckGo 등)을 가리킨다. 이는 큰 문제를 해결하는 복잡한 시스템이다:

1. **Crawling** – 웹을 탐색하고 모든 웹사이트의 콘텐츠를 구문 분석하는 프로세스이다. 사용 가능한 [도메인이 3억 5천만 개](https://www.businesswire.com/news/home/20200528005832/en/Internet-Grows-to-366.8-Million-Domain-Name-Registrations-at-the-End-of-the-First-Quarter-of-2020)가 넘으므로 이는 엄청난 작업이다.
2. **Indexing** - 액세스할 수 있도록 크롤링 단계에서 수집된 모든 데이터를 저장할 위치를 찾는다.
3. **Rendering** – 사이트의 기능을 향상시키고 콘텐츠를 풍부하게 하는 JavaScript와 같은 페이지의 모든 리소스를 실행한다. 이 프로세스는 크롤링되는 모든 페이지에 발생하지 않고 경우에 따라 콘텐츠가 실제로 인덱싱되기 전에 발생한다. 작업을 수행할 리소스가 없는 경우엔 인덱싱 후 렌더링이 발생할 수 있다.
4. **Ranking** – 데이터를 쿼리해서 검색 기반 관련 결과 페이지를 만든다. 검색 엔진에 다양한 순위 기준이 적용되어 사용자의 의도를 충족할 수 있는 최상의 결과를 제공한다.

Googlebot은 Google의 인터넷 크롤러로, 검색 결과를 제공하기 위한 콘텐츠 데이터베이스를 만드는 데 필요한 모든 정보를 수집하는 검색 시스템의 일부이다.

<br />

## 4. What are Web Crawlers?

검색 엔진(Google, Bing, Yandex, Baidu, Naver, Yahoo 또는 DuckDuckGo)은 웹 크롤러를 사용하여 웹 사이트와 해당 웹 페이지를 탐색해서 검색 결과를 만든다.

검색 엔진마다 국가마다 [시장 점유율](https://gs.statcounter.com/search-engine-market-share)이 다르다.

대부분의 국가에서는 가장 큰 검색 엔진인 Google 점유율이 가장 높다. 하지만 [중국](https://gs.statcounter.com/search-engine-market-share/all/china), [러시아](https://gs.statcounter.com/search-engine-market-share/all/russian-federation), [일본](https://gs.statcounter.com/search-engine-market-share/all/japan) 또는 [한국](https://gs.statcounter.com/search-engine-market-share/all/south-korea)의 경우 다른 검색 엔진과 지침을 확인해봐야한다.

Ranking 및 Rendering과 관련하여 약간의 차이가 있지만 대부분의 검색 엔진은 크롤링 및 인덱싱과 관련하여 유사한 방식으로 작동한다.

웹 크롤러는 사용자를 에뮬레이션해서 웹 사이트에서 찾은 링크를 탐색하여 페이지를 색인화하는 일종의 봇이다. 웹 크롤러는 [user-agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)를 커스텀해서 자신을 식별한다. Google에는 [여러 웹 크롤러](https://developers.google.com/search/docs/advanced/crawling/overview-google-crawlers)가 있는데 **Googlebot Desktop**과 **Googlebot Smartphone**이 자주 사용된다.

### 4.1. How Does Googlebot Work?

#### `Googlebot이 웹페이지 색인을 생성하는 과정`

![googlebot](./images/googlebot.avif)

프로세스의 일반적인 개요는 다음과 같다:

1. **URL 찾기**: Google은 [Google Search Console](https://search.google.com/search-console/welcome), 웹사이트 간 링크 또는 [XML 사이트맵](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview)을 비롯한 여러 위치의 URL을 가져온다.

2. **크롤링 대기열에 추가**: URL은 Googlebot이 처리할 크롤링 대기열에 추가된다. 크롤링 대기열의 URL은 일반적으로 몇 초 동안 지속되지만 경우에 따라 특히 페이지를 렌더링, 인덱싱해야 하거나 URL이 이미 인덱싱된 경우, 새로 고쳐야 하는 경우 최대 며칠이 걸릴 수 있다. 그리고 페이지가 [렌더링 대기열](https://nextjs.org/learn/seo/rendering-and-ranking)에 들어간다.

3. **HTTP 요청**: 크롤러는 헤더를 가져오기 위해 HTTP 요청을 하고 반환된 상태 코드에 따라 작동한다:

   - 200 - HTML을 크롤링하고 구문 분석한다.
   - 30X - 리디렉션을 따른다.
   - 40X - 오류를 기록하고 HTML을 로드하지 않는다.
   - 50X - 상태 코드가 변경되었는지 나중에 다시 확인한다.

4. **렌더링 대기열**: 검색 시스템의 다양한 서비스 및 구성 요소가 HTML을 처리하고 콘텐츠를 구문 분석한다. 페이지에 일부 JavaScript 클라이언트 기반 콘텐츠가 있는 경우 URL이 렌더링 대기열에 추가될 수 있다. 렌더링 대기열은 JavaScript를 렌더링하는 데 더 많은 리소스를 사용해야 하기 때문에 Google은 더 많은 비용이 든다. 그래서 렌더링된 URL은 인터넷의 전체 페이지에서 차지하는 비율이 적다. 일부 다른 검색 엔진은 Google과 동일한 렌더링 용량을 가지고 있지 않을 수도 있어서 Next.js가 렌더링 전략에 도움을 줄 수 있다.

5. **색인 생성 준비 완료**: 모든 기준이 충족되면 페이지가 색인화되고 검색 결과에 표시될 수 있게된다.

> **추가 자료**
>
> - Google: [SEO Starter Guide](https://developers.google.com/search/docs/fundamentals/seo-starter-guide)
> - MDN: MDN: [User-Agents](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/User-Agent)
