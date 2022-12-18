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
