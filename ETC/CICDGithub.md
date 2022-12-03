# CI/CD with Github

> 현재 진행중인 프로젝트 [이케해여](https://github.com/ChoJinmok/ikehaeyeo)에서 `Github Actions`로 `CI/CD`를 구현하면서 알게된 것들을 정리했습니다.

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

<br />

## 2. Github Actions로 CD 구현해보기

- `Vercel`을 좀 더 자유롭게 다루려면 GUI로는 해결할 수 없고 [Vercel CLI를 사용했어야했다](https://vercel.com/guides/using-vercel-cli-for-custom-workflows).

- CLI를 사용하려면 결국엔 툴을 사용했어야했는데 조금이지만 기존에 간단하게 다뤄봤던 `Github Actions`를 활용해서 `Vercel`배포를 하기로 했다.

- 다행히 `Vercel`에서 `Github Actions`를 `Vercel`과 함께 사용하는 [가이드](https://vercel.com/guides/how-can-i-use-github-actions-with-vercel)를 제공해주고 있었다.

- 아래에 나오는 주의사항만 잘 확인한다면 `Github Actions`로도 배포가 잘 되는 것을 확인할 수 있다.

### 가이드에 나와있지 않은 주의사항

#### **Vercel 설정 불러오기**

```yaml
- name: Pull Vercel Environment Information
  run: vercel pull --yes --environment=preview --token=${{ secrets.VERCEL_TOKEN }}
```

- 위의 코드는 `Vercel` GUI상에 설정해놓은 'Build & Development Settings', 'Environment Variables' 등을 가지고오는 작업이다.
- 그렇기 때문에 위의 코드를 그대로 사용하려면 GUI상 설정이 이루어져있어야한다.
- 만약 설정을 `pull`해오지 않는다면 `vercel.json`에 따로 설정을 해줘야할 것이다.(실제로 해보지는 않았다...)

> `Github`의 `Marketplace`에 가면 [Vercel Action](https://github.com/marketplace/actions/vercel-action?version=v25.1.0)을 찾을 수 있는데 문서를 보고 그대로 따라해도 `Github Actions`환경에서 vercel initial단계를 못벗어나는 문제를 만났다.  
> `Github`의 `Marketplace`에 [Deploy to Vercel Action](https://github.com/marketplace/actions/deploy-to-vercel-action?version=v1.9.10)같은 경우엔 `vercel.json`에서 많은 설정을 더 해줘야했다.  
> 그리고 위의 두 패키지(?)같은 경우엔 외부 패키지에 의존하는 것이어서 `Vercel`에서 제공하는 공식적인(?) 방법으로 해결하고 싶었다.
