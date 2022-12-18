# CSS hover test

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 hover style을 테스트할 수 있을까?를 찾아보면서 알게된 점을 정리했습니다.

<br />

결론부터 바로 말하자면 'hover state를 JavaScript로 테스트 할 수 없는 것 같다'이다. 같다고 말한 것은 아직 결론이 안났기 때문...

## 1. event를 발생시키면 hover가 되지 않을까?

- 프론트엔드의 여러 unit test library들은 이벤트를 발생시키는 방법들을 제공해주는데 테스트 환경에서 이벤트를 발생시키면 hover state를 테스트하는 것을 시도했다.
- 그런데 검색을 하면서 알게된 것이 브라우저에서 발생한 hover를 JavaScript가 알 수 없다고 하는 정보들을 많이 찾을 수 있었다. ([참고자료 1](https://github.com/testing-library/jest-dom/issues/59), [참고자료 2](https://groups.google.com/g/jasmine-js/c/SjnIoSpnx7g), 그런데 참고자료에서 확실하게 말하지는 않는다.)
- 확실하지는 않지만 상당히 생각해볼만한 문제였다. 근본적으로 `:hover`와 같은 css 선택자로 일어난 상태를 JavaScript에서 인지할 수 없다는 것이다.
- 그리고 Test Library에서 `fireEvent`는 이벤트 핸들러의 호출 정도를 가능하게 해주고 실제로 브라우저에서 일어나는 모든일을 대변하지도 않고 그럴 필요도 없어보인다.

## 2. 해결책

- CodeceptJS와 같은 E2E 테스트를 통해서 직접 브라우저에서 테스트 해주는 방법이 있을 것 같다.
- Unit test에서 테스트를 해주고 싶다면 리액트로 예를 들면 상태에 따라 스타일이 변하게 만들어서 이를 테스트 해줄 수 있을 것 같다.
