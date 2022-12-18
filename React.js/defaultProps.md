# React의 defaultProps

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 props에 직접 매개변수의 기본값을 지정해줬는데도 에러가 발생해서 해결하는 과정에서 알게된 것을 정리했다.

<br />

먼저 주제를 React가 아닌 TypeScript로 선정한 이유는 React의 코드가 아닌 컴포넌트의 props 타입을 설정한 부분에서 에러가 발생하고 리액트 관련코드에서는 에러가 발생하지 않았기 때문이다.

## 1. 상황

```tsx
interface SignUpFieldControllerProps {
  id: string;
//   type?: string; -> propType "type" is not required, but has no corresponding defaultProps declaration. 에러 발생
  name: string;
  value: string;
//   placeholder?: string; propType "placeholder" is not required, but has no corresponding defaultProps declaration. 에러 발생
  onChange: () => void;
}

export default function SignUpFieldController({
  id,
  type = "text",
  name,
  value,
  placeholder,
  onChange,
}: SignUpFieldControllerProps) { ... }
```

- 위와 같은 코드에서 type과 placeholder는 선택적으로 들어오는 prop이고 아무값도 전달되지 않는다면 type은 text, placeholder은 그냥 undefined가 들어오면 되는 상황이어서 type에 매개변수 기본값을 전달해주었다.

- 그런데 interface 부분에서 위와 같은 에러가 발생했다.

## 2. 해결

```tsx
interface SignUpFieldControllerProps {
  id: string;
  type?: string;
  name: string;
  value: string;
  placeholder?: string;
  onChange: () => void;
}

export default function SignUpFieldController({
  id,
  type,
  name,
  value,
  placeholder,
  onChange,
}: SignUpFieldControllerProps) { ... }

SignUpFieldController.defaultProps = {
  type: 'text',
  placeholder: undefined,
};
```

- 위와 같이 매개변수 부분이 아닌 defaultProps에서 기본값을 지정하니 에러가 사라졌다.

- 코드의 진행상으로는 에러가 날 상황은 보이지 않아서 eslint의 문서를 확인해봤다.

## 3. Enforce a defaultProps definition for every prop that is not a required prop ([`react/require-default-props`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/require-default-props.md))

코드에서 사용자 정의 기본 논리에 비해 `defaultProps`의 장점 중 하나는 `PropTypes` 타입 검사가 발생하기 전에 `defaultProps`가 React에 의해 해결되므로 typechecking이 defaultProps에도 적용된다는 것이다. 상태 비저장 기능 component에 대해서도 마찬가지이다. 기본 함수 매개 변수는 `defaultProps`와 동일하게 작동하지 않으므로 `defaultProps`를 사용하는 것이 좋다.

With `defaultProps`:

```jsx
const HelloWorld = ({ name }) => (
  <h1>
    Hello, {name.first} {name.last}!
  </h1>
);

HelloWorld.propTypes = {
  name: PropTypes.shape({
    first: PropTypes.string,
    last: PropTypes.string,
  }),
};

HelloWorld.defaultProps = {
  name: "john",
};

// Logs:
// Invalid prop `name` of type `string` supplied to `HelloWorld`, expected `object`.
ReactDOM.render(<HelloWorld />, document.getElementById("app"));
```

Without `defaultProps`:

```jsx
const HelloWorld = ({ name = "John Doe" }) => (
  <h1>
    Hello, {name.first} {name.last}!
  </h1>
);

HelloWorld.propTypes = {
  name: PropTypes.shape({
    first: PropTypes.string,
    last: PropTypes.string,
  }),
};

// 아무것도 기록되지 않고 렌더링된다:
// "Hello,!"
ReactDOM.render(<HelloWorld />, document.getElementById("app"));
```

## 결론

- Eslint 문서에서 알 수 있듯 defaultProps는 타입 확인이 가능하고 매개 변수 기본값은 타입 확인이 되지 않고 통과하는 것을 볼 수 있었다.

- 그러나 propTypes이 아닌 TypeScript로 타입 확인을 하면 위와 같은 에러를 미리 다 잡아주는 것으로 보인다.

- 검색을 해보니 class형 컴포넌트에서 defaultProps가 필요해 보이며 TypeScript와 함께사용하는 함수형 컴포넌트에서는 필수는 아니라고 생각된다.
