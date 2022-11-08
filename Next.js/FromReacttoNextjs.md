# From React to Next.js

> Next.js의 튜토리얼 중 [From React to Next.js](https://nextjs.org/learn/foundations/from-react-to-nextjs)를 보고 배우고 느낀점을 정리했습니다.

### 1. 새롭게 알게된 점

- 노마드 코더의 강의를 들었는데도 `index.html`가 없는지 파악하지 못했다. => Next.js가 `<html>`, `<body>`태그들을 자동으로 생성해준다.

### 2. 특징

- ReactDOM.render()도 Next.js가 자동으로 해준다.
- Babel이 아마도(?) Next.js에 내장되어 있어서 브라우저가 이해할 수 있게 JS로 컴파일해준다.
- 화면에 렌더링할 페이지들은 `pages`라는 디렉토리에 담아줘야한다.(나중에 더 자세히 설명해준다고 한다.)
- pages에 있는 컴포넌트를 export default해주면 next.js가 자동으로 구별하여 그 페이지를 레더링해준다.
- 개발 환경에서 서버를 실행해보고 싶으면 `next dev`를 스크립트에 추가해서 실행한다.
  [Fast Reresh](https://nextjs.org/docs/basic-features/fast-refresh)기능이 있어서 코드를 변경하고 저장하면 수정 사항이 자동으로 반영 업데이트 된다.(Next.js로 pre-configured돼서 제공되는 것)

### 3. 장점

- React는 modern interactive UI를 빌드하기 위한 유용한 라이브러리이지만 페이지들을 결합하는 과정은 따로 해주어야한다. => Next.js가 아주 편리하게 페이지들을 결합해준다.
- 복잡한 도구인 babel 스크립트를 제거함으로써 더 이상 babel에대해 생각할 필요가 없어졌다.
- **Fast Refresh**
