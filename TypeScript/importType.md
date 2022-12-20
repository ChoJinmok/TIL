# import type

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 TypeScript의 type을 import해오는 상황에서 eslint의 `import/no-cycle`에러를 만나고 해결하는 과정에서 알게된 것들을 정리했다.

<br />

## type은 `import type`으로

- 상황은 이렇다. redux에서 slice -> rootReducer -> store 순으로 import되고 있는데 store에서 만든 Thunk Action의 타입을 다시 slice에서 import해오면서 eslint의 `import/no-cycle`에러가 발생했다.

- 결국은 store에 모여서 만들어지는 타입을 다른곳에서도 사용해야하는 상황이어서 무조건 순환 의존성 에러가 발생할 수 밖에 없는 상황이었다.

- 그런데 검색 중 발견한 [redux-toolkit의 github의 issue](https://github.com/reduxjs/redux-toolkit/issues/2390)에 같은 문제로 누군가 질문을 했고, redux팀에서의 답변을 발견했다.

- 설명은 다음과 같다. TypeScript의 타입은 "순환의 방향"이 달라서 순환 의존성 에러가 발생하지 않는다.
