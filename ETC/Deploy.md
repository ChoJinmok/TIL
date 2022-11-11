# 배포

> firebase를 연습한 프로젝트를 배포하는 과정에서 생긴 문제점과 해결과정 (따로 프레임워크 preset을 사용하지 않고 webpack으로만 프로젝트 진행)

# 1. GitHub pages 배포

## 1 - 문제: main.js가 어딨어?

- 수동으로 만들었는데도 빌드 후 배포환경에서 main.js를 불어오지 못하는 상황이었다.

## 1 - 원인: 경로가 잘못됐어!

- chrome의 개발모드 network에서 HTML의 script태그에서 src의 경로가 문제인 것을 파악했다.
- GitHub pages는 기본적으로 GitHub주소뒤에 저장소명이 붙는 방식으로 배포 도메인 이름이 정해진다. (`https://chojinmok.github.io/firebasepractice`)
- 개발환경에서는 src의 주소가 `/main.js`(현재 디렉토리에 있는 main.js)를 가지고 오면 되는 상황이었지만 배포환경에서는 `/firebasepractice/main.js`에서 JavaScript파일을 받아와야하는 상황이었다.

## 1 - 해결: main.js는 여깄지!

- 처음에는 webpack의 설정을 어떻게 해결할지 몰랐기도 했고 내가 생각한 원인을 해결하면 문제가 해결되는지 알고 싶어서 수동으로 index.html의 script src경로를 바꿔줬다. => **main.js 불러오기 성고!!**

<br />

## 2 - 문제: 다 끝난 줄 알았지?

- main.js를 잘 불러왔는데 footer만 화면에 나오는 상황이었다. 에러가 콘솔창에 찍히지 않아 원인을 바로 알 수 있었던 처음과 달리 원인을 찾기가 힘들었다.

## 2 - 원인: url을 보고 컴포넌트를 못불러오는데..? 혹시 react router dom..?

- App 컴포넌트의 footer부분은 화면에 잘 나오는 걸 보면 Router가 문제가 있을 수 있겠다는 결론에 겨우 이르렀다.
- 실제로 찾아보니 GitHub pages 경로 이슈로 react router dom의 경로도 수정해줘야한다는 정보가 많았다.
- 당연하게도 첫번째 문제에 연장선으로 베이스 경로가 달라지니 Router의 베이스도 `/`가 아닌 `/firebasepractice/`가 되어야하는 것이었다.

## 2 - 해결: BrowserRouter의 basename!

- HashRouter를 사용해서 문제를 해결하는 방법도 있었지만 HashRouter는 편하긴 하지만 여러가지 단점이 있어서 찾아보니 BrowserRouter에 basename을 index.html의 script src경로와 일치시켜주면 해결되는 문제였다.

<br />

## 3 - 문제: webpack아 니가 자동으로 해주면 안될까?

- 문제는 해결됐지만 webpack을 통해 build를 했을 때 React.js코드가 JavaScript로 잘 번들됐지만 index.html을 수동으로 만들어주고 script src경로도 수동으로 작성해줘야하고 BrowserRouter에 basename도 수동으로 바꿔줘야하는 상황이었다.

## 3.1 - 해결: "나 혼자선 힘들고 HtmlWebpackPlugin이라고 있어 걔좀 불러줘"

- webpack의 plugin은 번들된 결과물을 한번 더 처리해주는 역할을 하는데 HtmlWebpackPlugin은 그 중 HTML 파일을 후 처리 해준다.
- templage에는 처리해줄 HTML파일의 경로를 넣어주고, publicPath로 script태그의 src의 기본 경로를 정해줄 수 있었다. (templateParameters로 HTML파일에 매개변수를 넘겨주고 사용까지 할 수 있게 해준다.)
  ```javascript
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./public/index.html"),
      publicPath: "/firebasepractice",
    }),
  ];
  ```
- 이렇게 해주면 build 결과물 디렉터리에 HTML결과물도 후 처리돼서 만들어진다.

## 3.2 - 해결: "아 DefinePlugin 얘도 필요하겠다!"

- Router의 basename와 script태그의 src의 기본 경로를 수동으로 바꿔주는 것이 아닌 개발 환경에 따라 정해주고 싶었다.
- development mode인지 poduct mode인지는 webpack에서 결정되므로 webpack의 mode에 따라서 경로를 바꿔줄 수 있으면 좋겠다고 생각했다.
- 찾아보니 process.env.NODE_ENV라는 환경변수를 webpack이 자동으로 만들어줘서 사용할 수 있지만 dotenv를 webpack에서 환경변수에 덮어씌워주는 바람에 사용하지 못했다.(더 쉬운방법)
- 다른 방법을 찾아보니 webpack을 export할 때 함수를 export할 수 있는데 두번째 매개변수(argv)에 mode라는 property가 담겨져 있었다.
- DefinePlugin을 사용하면 환경변수를 만들어서 사용할 수 있는데 `NODE_ENV`라는 환경변수에 `argv.mode`를 담아주면 Router에서 받아서 사용하면 해결될 거 같았다.

  ```javascript
  // webpack.config.js
  module.exports = (_, argv) => {
    // ...

    plugins: [
      new DefinePlugin({
        NODE_ENV: JSON.stringify(argv.mode),
      }),
    ];
  };
  ```

  ```javascript
  // Router.jsx

  <BrowserRouter basename={NODE_ENV ? '/' : '/firebasepractice'}>
  ```

- 또 다른 방법으로 직접 해보진 않았지만 package.json hompage를 설정해주면 `process.env.PUBLIC_URL`이라는 환경변수에 url이 담겨서 Router의 basename에 넣어주면된다.

- HTML의 script태그의 src 기본 경로도 같은 방법을 적용할 수 있었다.

  ```javascript
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./public/index.html"),
      publicPath: argv.mode ? "/" : "/firebasepractice",

      // 개발 환경에 따른 매개변수도 전달 가능
      templateParameters: {
        base: argv.mode ? "/" : "/firebasepractice",
      },
    }),
  ];
  ```

<br />

## 4 - 문제: 새로고침 눌러볼까? 엥? 왜 안돼?

- 잘 작동하는 줄 알고 좋아했는데 새로고침을 하니 Not Found Page... 404에러가 떴다.

## 4 - 원인: pushState history API?? 난 그런거 모르겠고!

- github.io는 HTML5 pushState history API를 지원하지 않는다. 그런데 BrowserRouter는 pushState API를 사용한다...
  - pushState API는 페이지를 reload하지 않고 주소만 변경할 때 사용하는 방식, SPA에서 많이 사용된다.
- 내가 만든 어플리케이션의 첫 페이지인 홈의 url이 아닌곳에서 새로고침을 하면 다시 `index.html`로 돌아와서 어플리케이션이 구동돼야하는데 그 위치에서 html을 찾으려고 해서 `index.html`을 브라우저가 찾지 못하는 문제 였다.
  - 만약 `https://chojinmok.github.io/firebasepractice/profile`이라는 경로에서 새로고침을 해도 `/firebasepractice/index.html`을 다시 새로고침해야하는데 `/firebasepractice/profile/index.html`에서 html을 찾으니 에러가 발생했다.
- 찾아보니 결국 HashRouter를 사용하라는 말이 많았다.
- HashRouter는 끝끝내 사용하기 싫어서 찾아보니 꼼수가 있었다. GitHub Pages는 404에러가 뜨면 GitHub의 Not Found Page를 여는데 404 페이지를 404.html을 만들어서 커스터마이징해줄 수 있는 것이었다.
- 404.html의 script에 해당 URL을 쿼리로 만들어서 리다이렉션하면 index.html에서 변경된 URL을 받고 해당 쿼리를 해석해서 올바른 URL로 변경하는 방법이 있었다. [링크](https://iamsjy17.github.io/react/2018/11/04/githubpage-SPA.html)
- 하지만 결국 수동으로 조작해줘야했다... 그래서 또 찾은 꼼수가 index.html과 똑같은 내용을 가진 404.html을 만드는 것이다. 결국 webpack의 devserver에 historyApiFallback을 사용한 것과 똑같은 원리이다. (historyApiFallback에 true를 주면 404에러가 떴을 때 index.html로 돌아가게 된다.) -> 그런데 404.html을 사용하면 SEO에 좋지 않다고 한다.([링크](https://sujinlee.me/spa-github-pages-ko/)) 위의 방법으로 하면 SEO가 검색 모두 반영

  ```javascript
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./public/index.html"),
      publicPath: argv.mode ? "/" : "/firebasepractice",
    }),
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./public/index.html"),
      publicPath: argv.mode ? "/" : "/firebasepractice",
      filename: "404.html",
    }),
  ];
  ```

- HtmlWebpackPlugin을 두번사용해서 404.html파일을 생성했다.

<br />

## 5 - 문제: 빌드 완료!! 그런데 배포는?

- 빌드 완료 후 GitHub repo의 Pages setting을 통해서 쉽게 해줄 수 있었지만 gh-pages 패키지를 사용했을 때처럼 ph-pages branch에 빌드한 파일만 적용시켜서 배포시키고 싶었다.
- 그런데 main branch와 독립된 branch는 어떻게 만드는지..?

## 5 - 해결: orphan 옵션을 이용!

1. `git checkout -–orphan gh-pages`로 gh-pages 브랜치를 만든다. (orphan 옵션을 사용하면 부모 없는 브랜치를 만든다.)
2. `npm run build`를 실행해서 build파일들 만든다.
3. `gir rm -rf .`를 이용해서 프로젝트 파일들 삭제한다.
4. build 결과물 디렉토리 안의 파일들을 밖으로 꺼내서 빌드 파일들만 git add -> commit -> push를 해준다.

## 5 - 한계: 결국 수작업으로..

- 현재까지는 여기까지 완료한 상황이다. 처음부터 빌드까지도 수작업 후 자동화하는 방법을 찾았었는데 배포도 언젠가 자동화로 할 수 있지 않을까?? ㅠ.ㅠ
- gh-pages 패키지를 이용하면 아마도 자동화가 가능하지 않을까 싶다. ([gh-pages](https://www.npmjs.com/package/gh-pages), [GitHub Actions로 gh-pages 배포 자동화](https://davidyang2149.dev/front-end/github-actions%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-gh-pages-%EC%9E%90%EB%8F%99-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/))

<br />

# 2. vercel 배포

## 1. GUI로 쉽게!

- GUI조작으로 쉽게 GitHub Repo에 접근해서 배포를 할 수 있다.
- 어느 Repo에 프레임워크 프리셋, build를 어떻게 시키는지 정도만 알고 있으면 쉽게 배포할 수 있다.
- GitHub Pages와 다르게 SPA도 지원한다.

## 2. 그런데 왜 인증이 안되지?

- firebase 콘솔과 google의 프로젝트 인증까지 모두 세팅해줬는데 소셜 로그인이 동작되지 않았다.
- 환경변수를 vercel에 넘겨주지 않았기 때문에 발생한 문제였다. vercel을 처음에 배포할 때도 설정해줄 수 있지만 배포 후에서 따로 환경변수를 세팅해줄 수 있다.

## 3. 여기서도 새로고침하면 404..?

- 별도의 설정을 해주지 않으면 url 변경후 새로고침하면 vercel에서도 404페이지가 뜬다.
- `vercel.json` 생성후 rewrite을 작성해주면 해결!
  ```json
  {
    "rewrites": [
      {
        "source": "(.*)",
        "destination": "/index.html"
      }
    ]
  }
  ```

## 4. 배포 자동화?

- 기본적으로 내가 설정해놓은 branch를 자동으로 감시하고 배포하는 것 같다. 그리고 branch를 merge하기 전 제대로 돌아가는 지 확인할 수 있게 **Preview**로 배포를 해준다.
- 추 후 **CI/CD** 더 파보기! ([링크1](https://velog.io/@tnqls1211v/TIL-Day72), [링크2](https://monicareport.tistory.com/487))
