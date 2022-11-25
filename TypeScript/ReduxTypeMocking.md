# Redux의 useSelector, useDispatch type mocking

> Redux를 사용하면서 Jest 테스트 코드를 짜게되면 useSelector, useDispatch 등을 mocking해주야하는 상황이 발생하는데 이번에 TypeScript까지 적용시키면서 생긴 문제점을 정리했습니다.

<br />

## 1. 문제 발생

```typescript
// __mocks__/react-redux.ts

export const useDispatch = jest.fn();

export const useSelector = jest.fn();
```

```typescript
// container.test.ts

import { useDispatch, useSelector } from "react-redux";

jest.mock("react-redux");

test("test", () => {
  const dispatch = jest.fn();
  const state = {
    //...
  };

  // 아래 두 코드에서 에러 발생
  useDispatch.mockImplementation(() => dispatch);

  useSelector.mockImplementation((selector) => selector(state));
});
```

- Jest는 jest.mock을 사용하면 mocking한 useDispatch와 useSelector을 import해오지만 TypeScript는 원래 useDispatch와 useSelector의 타입을 참조하고 있기 때문에 mockImplementation이라는 property가 없다고 에러가 발생한다.

## 2. 해결 방법

- as를 사용해서 useDispatch와 useSelector가 mocking한 함수임을 알려준다.

```typescript
import { useDispatch, useSelector } from "react-redux";

jest.mock("react-redux");

const mockedUseDispatch = useDispatch as jest.Mock<typeof useDispatch>;
const mockedUseSelector = useSelector as jest.Mock<typeof useSelector>;

test("test", () => {
  const dispatch = jest.fn();
  const state = {
    //...
  };

  // 아래 두 코드에서 에러 발생
  mockedUseDispatch.mockImplementation(() => dispatch);

  mockedUseSelector.mockImplementation((selector) => selector(state));
});
```
