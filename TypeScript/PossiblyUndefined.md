# TypeScript possibly undefined value 에러

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 TypeScript를 적용하면서 만난 문제를 해결하면서 알게된 점을 정리했다.

<br />

## 1. 상황

```ts
const signUpField = signUpFields.find(({ name }) => name === signUpFiledName);

// signUpField.label -> 에러 발생
```

## 2. 원인

- `find` 메서드로 특정 요소를 찾을 수도 있지만 못찾을 경우 signUpField가 `undefined`가 될 수도 있어서 에러가 발생했다.

## 3. TypeScript possibly undefined value 에러 해결방법

1. 조건문을 활용해서 에러가 발생할 수 있는 타입이 들어올 경우 예외 처리
2. OR 연산자 혹은 Null 병합 연산자 사용
3. Non-null assertion operator 사용
   - Non-null assertion operator: 접미에 느낌표(!) 연산자를 붙여서 null, undefined가 아니라고 단언해준다.
   - ex. `signUpField!`
4. `as` 사용해서 type 단언

## 4. 해결

- `as` 사용해서 해결

```ts
const signUpField = signUpFields.find(
  ({ name }) => name === signUpFiledName
) as SignUpField;

signUpField.label;
```

- `as`를 사용해서 TypeScript 에러를 해결할 순 있지만 실제로는 `undefined`가 signUpField로 들어올 수 있는 상황이어서 근본적인 해결책이 아니라고 생각이 들었다.

- 테스트 코드 중 일부이기 때문에 error를 발생시키는 객체를 만들고 `undefined`가 들어올 경우 null 병합 연산자를 활용해서 error 객체가 singUpField에 담기게 코드를 작성해서 에러를 해결했다.

```ts
const notFoundFiled: SignUpFieldType = {
  name: "name",
  label: "error",
};

const signUpField =
  signUpFields.find(({ name }) => name === signUpFiledName) ?? notFoundFiled;

signUpField.label;
```

## 5. Reference

- [How to solve TypeScript possibly undefined value](https://linguinecode.com/post/how-to-solve-typescript-possibly-undefined-value)
