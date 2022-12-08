# Pre-rendering and Data Fetching

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Pre-rendering and Data Fetching](https://nextjs.org/learn/basics/data-fetching)내용들을 정리했습니다.

<br />

우리는 블로그 콘텐츠를 파일 시스템에 저장하지만 콘텐츠가 데이터베이스나 [Headless CMS](https://en.wikipedia.org/wiki/Headless_content_management_system)에 저장되어 있어도 작동한다.

<br />

## 1. Pre-rendering

[데이터 가져오기](https://nextjs.org/docs/basic-features/data-fetching/overview)에 대해 이야기하기 전에 Next.js에서 가장 중요한 개념 중 하나인 [사전 렌더링](https://nextjs.org/docs/basic-features/pages#pre-rendering)에 대해 이야기해 보겠다.

기본적으로 Next.js는 모든 페이지를 사전 렌더링한다. 즉, 클라이언트의 JavaScript에서 모든 작업을 수행하지 않고 Next.js는 미리 각 페이지에 대한 HTML을 생성한다. 사전 렌더링을 통해 더 나은 성능과 [SEO](https://en.wikipedia.org/wiki/Search_engine_optimization)를 얻을 수 있다.

생성된 각 HTML은 해당 페이지에 필요한 최소한의 JavaScript 코드와 연결된다. 페이지가 브라우저에 의해 로드되면 해당 JavaScript 코드가 실행되어 페이지가 완전히 상호 작용하도록 한다. (이 과정을 **hydration**라고 합니다.)

### 1.1. 사전 렌더링이 진행 중인지 확인

다음 단계를 수행하여 사전 렌더링이 진행되고 있는지 확인할 수 있다.

- 브라우저에서 JavaScript를 비활성화하고([Chrome에서 방법 참조](https://developer.chrome.com/docs/devtools/))…
- [이 페이지에 액세스](https://next-learn-starter.vercel.app) (이 튜토리얼의 최종 결과).

앱이 JavaScript 없이 렌더링되는 것을 볼 수 있다. 이는 Next.js가 앱을 정적 HTML로 사전 렌더링했기 때문에 JavaScript를 실행하지 않고도 앱 UI를 볼 수 있다.

> localhost에서 위의 단계를 시도할 수도 있지만 JavaScript를 비활성화하면 CSS가 로드되지 않는다.

앱이 일반 React.js 앱(Next.js 없음)인 경우 [사전 렌더링](https://nextjs.org/docs/basic-features/pages#pre-rendering)이 없으므로 JavaScript를 비활성화하면 앱을 볼 수 없다. 예를 들어:

- 브라우저에서 JavaScript를 활성화하고 [이 페이지를 확인](https://create-react-template.vercel.app/). 이는 [Create React App](https://create-react-app.dev)으로 빌드된 일반 React.js 앱이다.
- 이제 JavaScript를 비활성화하고 [동일한 페이지](https://create-react-template.vercel.app)에 다시 액세스
- 앱이 더 이상 표시되지 않는다. 대신 "이 앱을 실행하려면 JavaScript를 활성화해야 합니다."라고 표시된다. 이는 앱이 정적 HTML로 미리 렌더링되지 않기 때문이다.

### 1.2. Summary: Pre-rendering vs No Pre-rendering

![pre-rendering](./images/pre-rendering.png)
![no-pre-rendering](./images/no-pre-rendering.png)
