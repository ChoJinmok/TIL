# Create a Next.js App

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Create a Next.js App](https://nextjs.org/learn/basics/create-nextjs-app)내용들을 정리했습니다.

<br />

처음부터 React로 완전한 웹 애플리케이션을 구축하려면 고려해야 할 세부 사항이 많다:

- 코드를 webpack과 같은 번들러를 사용하여 번들링하고 Babel과 같은 컴파일러를 사용하여 변환해야 한다.
- Code splitting과 같은 production 최적화를 수행해야 한다.
- 성능 및 SEO를 위해선 일부 페이지를 정적으로 pre-rendering 해야한다. 즉, 서버 사이드 렌더링이나 클라이언트 사이드 렌더링을 고려해야 한다.
- React 앱을 데이터 저장소에 연결하기 위해 일부 서버 측 코드를 작성해야 할 수도 있다.

프레임워크는 이러한 문제를 해결할 수 있다. 그러나 추상화가 제대로 되어있지 않은 프레임워크는 그다지 유용하지 않을 수 있다. 또한 숙련되어야 좋은 코드를 작성할 수 있다.

<br />

## Next.js: The React Framework

Next.js는 앞서 말한 문제의 솔루션을 제공한다. 그리고 다음과 같은 기능을 제공하는 것을 목표로 한다:

- 직관적인 [페이지 기반](https://nextjs.org/docs/basic-features/pages) 라우팅 시스템([동적 경로](https://nextjs.org/docs/routing/dynamic-routes) 지원)
- 페이지별로 [사전 렌더링](https://nextjs.org/docs/basic-features/pages#pre-rendering), [정적 생성](https://nextjs.org/docs/basic-features/pages#static-generation-recommended)(SSG) 및 [서버측 렌더링](https://nextjs.org/docs/basic-features/pages#server-side-rendering)(SSR)을 지원한다.
- 더 빠른 페이지 로드를 위한 자동 코드 분할
- 최적화된 프리페치를 사용한 [클라이언트 사이드 라우팅](https://nextjs.org/docs/routing/introduction#linking-between-pages)
- [Built-in CSS](https://nextjs.org/docs/basic-features/built-in-css-support) 및 [Sass 지원](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support) 및 모든 [CSS-in-JS](https://nextjs.org/docs/basic-features/built-in-css-support#css-in-js) 라이브러리 지원
- [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh)를 지원하는 개발 환경
- Serverless Functions로 API endpoints을 구축하기 위한 [API routes](https://nextjs.org/docs/api-routes/introduction)
- 완전한 확장

## Setup ~ Editing the Page

- 설치 후 `npm run dev`를 실행하면 3000번 port에서 Next.js 앱의 "development server"(나중에 자세히 설명)가 시작된다.
- 코드를 수정하고 저장하면 새로 고침하지 않아도 자동으로 `http://localhost:3000`에 코드가 반영된다.
- Next.js 개발 서버에는 [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh)가 활성화되어 있다. 파일을 변경하면 Next.js가 자동으로 브라우저에 변경 사항을 거의 즉시 적용한다.
