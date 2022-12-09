# Keyof Type Operator

> > [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 TypeScript를 적용하면서 만난 문제를 해결하면서 알게된 점을 정리했다.

<br />

## 1. 상황

```ts
// store/modules/signUpSlice.ts

export interface SignUpFields {
  name: string;
  birthDate: string;
  phoneNumber: string;
  gender: string;
  streetNameAddress: string;
  detailedAddress: string;
  zipCode: number;
  email: string;
  password: string;
}

export interface SignUpState {
  signUpFields: SignUpFields;
}
```

```ts
// fixtures/signUpFields.ts

export interface SignUpField {
  name: string;
  label: string;
  type?: string;
}

const signUpFields: SignUpField[] = [
  { name: "name", label: "이름" },
  { name: "birthDate", label: "생일" },
  { name: "phoneNumber", label: "휴대폰", type: "tel" },
  { name: "gender", label: "성별 (선택 사항)" },
  { name: "streetNameAddress", label: "도로명 주소" },
  { name: "detailedAddress", label: "상세 주소" },
  { name: "zipCode", label: "우편번호", type: "number" },
  { name: "email", label: "이메일", type: "email" },
  { name: "password", label: "비밀번호", type: "password" },
];

export default signUpFields;
```

```tsx
// __tests__/SignUpContainer.test.tsx

import SIGN_UP_FIELDS from "../../fixtures/signUpFields";

const signUpState: SignUpState = {
  signUpFields: {
    name: "name",
    birthDate: "1991-11-27",
    phoneNumber: "010",
    gender: "male",
    streetNameAddress: "test",
    detailedAddress: "test",
    zipCode: 1234,
    email: "test@test",
    password: "1234",
  },
};

// ...

SIGN_UP_FIELDS.forEach(({ name, label }) => {
  // signUpFields[name]에서 에러 발생
  expect(signUpInput).toHaveValue(signUpFields[name]);
});
```

- 에러 코드
  - Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'SignUpFields'.
  - No index signature with a parameter of type 'string' was found on type 'SignUpFields'.ts(7053)

## 2. 원인

- 에러들을 보고 원인을 유추해보니 slice의 SignUpFields interface가 구체적으로 명시돼있어서 단순한 string type으로 signUpFields 객체에 접근하는 것이 옳지 않다고 알려주는 것 같았다.

## 3. 해결

- 그럼 fixture의 signUpFields의 name을 slice의 SignUpFields interface key들로 구체적인 타입을 지정해주면 해결될 것 같았다.
- 그래서 object type의 key를 타입으로 가지고올 수 있는 방법을 찾았고 다행히 [`keyof`](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)라는 타입 연산자가 존재했다.

```ts
// fixtures/signUpFields.ts

import { SignUpFields } from "../store/modules/signUpSlice";

export interface SignUpField {
  name: keyof SignUpFields;
  label: string;
  type?: string;
}
```

- 위와 같이 name의 타입을 구체적으로 명시해주니 해결됐다.
