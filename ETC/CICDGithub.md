# CI/CD with Github

> 현재 진행중인 프로젝트 [이케해여](https://github.com/ChoJinmok/ikehaeyeo)에서 `Github actions`로 `CI/CD`를 구현하면서 알게된 것들을 정리했습니다.

## 1. 상황

- 기존엔 `CI`만 `Github action`로 구현해놓은 상황이었다.

  - `CI`에서는 유닛 테스트, 테스트 커버리지 100%, E2E 테스트, 린트를 확인하게 끔 구현했다.
  - branch에서 PR, main에서는 push event에서 action이 실행되게끔 구현했다.

  ```yaml
  name: CI

    on:
        push:
            branches:
                - main
        pull_request:

    jobs:
    build:
        runs-on: ubuntu-latest

        strategy:
        matrix:
            node-version: [16.x]

        steps:
        - uses: actions/checkout@v3
        - name: Use Node.js ${{ matrix.node-version }}
            uses: actions/setup-node@v3
            with:
                node-version: ${{ matrix.node-version }}
        - name: Install
            run: npm ci
        - name: Test
            run: npm test
            env:
                HEADLESS: true
        - name: Lint
            run: npm run lint
  ```

- CD도 구현은 해놓은 상태였는데 `Vercel`에 GUI를 활용하면 `Vercel`이 등록해놓은 Github의 Repo를 감시하고 있다가 변화가 일어나면 자동으로 배포를 해주는 기능을 활용했다.

- `Vercel`에 Github의 Repo를 등록해놓으면 어떤 상황이던지 자동으로 배포를 해버린다. 그런데 `CI`가 성공적으로 완료되어야 `CD`로 넘어가서 배포를 하는 것이 맞다고 생각했고 가능하다면 배포후에 E2E테스트로 해줄 수 있으면 좋겠다고 생각했다.
