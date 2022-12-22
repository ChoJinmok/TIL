# Mocking useState

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 `useState`를 mocking해주면서 알게된 것을 정리했다.

<br />

## 1. `useDispatch`, `useSelector`와 유사하게!

- 코드숨에서 `useDispatch`, `useSelector`을 mocking해주는 방법을 배웠었는데 비슷하게 적용하면 되겠다고 생각했다.
- `setState`를 mock function으로 만들어주고 특정 이벤트가 발생했을 때 setState가 호출됐는지 확인하는 식으로 진행했다.

## 2. 진행 코드

```typescript
import { useState } from "react";

jest.mock("react", () => ({
  ...jest.requireActual("react"),
  useState: jest.fn(),
}));

const mockedUseState = useState as jest.Mock;

// ...

const setState = jest.fn();

mockedUseState.mockImplementation((initialState) => [initialState, setState]);
```

## 3. 대체 방법

- 찾아보니 많은 사람들은 `spyOn`을 사용해서 mocking을 해줬다.

```typescript
import { useState } from "react";

jest.mock("react", () => ({
  ...jest.requireActual("react"),
  useState: jest.fn(),
}));

// ...

const useStateSpy = jest.spyOn(React, "useState");

const setState = jest.fn();

mockedUseState.mockImplementation((initialState) => [initialState, setState]);
```

## 4. 추가 구현 사항

- 프로젝트도중 state가 현재 어떤 상태인가에 따라 화면이 어떻게 달라지는지를 테스트해야했었다.
- 그런데 위의 상황에서는 `initialState`르 따로 설정할 방법이 없다.
- 그래서 다음과 같은 방법을 생각했다.

  ```typescript
  const useStateInitialState = { ... }

  mockedUseState.mockImplementation(() => [useStateInitialState, setState]);
  ```

- 위와 같은 방법을 [`given2`](https://www.npmjs.com/package/given2)라이브러리와 함께 사용하면 테스트가 가능했다.

### 4.1. 한계점

- 위의 `mockImplementation`을 보면 실제 구현상황과 다른 것을 알 수 있다.
- 여전히 setState의 인자로 함수가 들어올 땐 Redux의 dispatch와 같이 `toBeCalled`로 단순히 불려진 정도만 테스트가 가능하다. (현재 어떤 상태 변경, 액션이 일어나는 지 확인하고 싶다.)
