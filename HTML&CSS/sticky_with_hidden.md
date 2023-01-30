# `position: sticky`가 `overflow: hidden`일때 왜 적용되지 않을까

> 회사에서 진행하고 있는 랜딩페이지에서 `position: sticky`가 적용안되는 문제를 만나서 해결하는 과정을 정리했습니다.

<br />

## 1. 상황

- `position: sticky`을 적용해야하는 상황에서 이상하게 동작하지 않았다.

- 구글링을 하다가 알게된 사실은 부모요소에 `overflow: hidden` 설정이 있는 경우 `position: sticky`가 제대로 동작하지 않는다는 것이다.

## 2. 이유

- `overflow: hidden`은 `position: sticky` 작동을 방해하지 않는다.

- 조상 요소의 `overflow: hidden` 설정은 조상 요소를 `sticky` 요소의 스크롤 컨테이너로 만든다.

- 스크롤 컨테이너는 `sticky` 요소가 붙는 컨테이너로 원래는 창 전체가 스크롤 컨테이너가 돼서 창에 붙어야하지만 `overflow: hidden` 설정을 준 요소가 스크롤 컨테이너가 되면서 요소에 붙어 있는 것이다.

- `sticky` 요소는 `relative` 요소와 유사하게 배치되지만 가장 가까운 스크롤 컨테이너, (스크롤 컨테이너 조상이 없는 경우) 뷰포트를 참조하여 계산된다.

## 3. 요소의 바깥 부분은 숨기면서 `sticky`를 적용해야한다면?

- `overflow: hidden`외에 `contain: paint`도 요소의 바깥 부분을 숨겨준다.

- `contain` 속성은 콘텐츠가 문서 트리의 다른 부분과 독립되어있음을 나타낼 때 사용한다.

- 문서 트리의 다른 부분과 독립되어있다는 것은 개발자가 스타일링을 주기 까다로워질 수 있다.

  - `paint`, `strict`, `content` 설정

    - 새로운 컨테이닝 블록 (position 속성이 absolute 또는 fixed인 자손을 위함)

    - 새로운 쌓임 맥락

    - 새로운 블록 서식 맥락

  => 이렇듯 새로운 설정이 되는 것은 기존에 알고 있던 CSS규칙이 꼬이는 것을 의미해서 주의해야한다!

## 4. 참고자료

- [mdn contain](https://developer.mozilla.org/ko/docs/Web/CSS/contain)
- [Why does `overflow:hidden` prevent `position:sticky` from working?](https://stackoverflow.com/questions/43909940/why-does-overflowhidden-prevent-positionsticky-from-working)
