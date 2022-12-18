# 빵 부스러기

> [한정수님의 TIL 중 빵 부스러기](https://github.com/Integerous/TIL/blob/master/ETC/BreadCrumbs.md)를 보고 감명을 받아서 작성하기 시작했습니다.  
> 저 뿐만 아니라 많은 분들도 '빵 부스러기'들을 모으셨으면 좋겠습니다.  
> 개발 관련 학습 중 하나의 글로 작성하기엔 짧고, 버리기엔 아까운 부스러기 정보들을 모아두는 곳입니다.

---

## 1. a태그 사용법?

- a태그는 div와 같은 컨테이너보다 텍스트를 감싸주는 것이 좋다? -> 더 찾아보기

---

## 2. Next.js의 warn: Fast Refresh will fall back to doing a full reload

- 해결을 못하다가 image에 priority넣어주니까 해결됐다... -> 더 찾아보기

---

## 3. Git에서 디렉토리명 변경하기

Git으로 버전관리를 하는동안 디렉토리명을 변경한 후 원격 저장소에 push를 하면 결과가 제대로 반영되지 않았다.

### 3.1. 원인

- Git은 OS단에서의 디렉토리명 변경을 변경사항ㅇ로 인식하지 않는다.

### 3.2. 해결방법

- `git mv oldName newName` 명령어로 Git에서 디렉토리명 변경을 인식하게 만든다.
- 그런데 `mv` 명령어는 대소문자를 구분하지 못한다. -> 임시로 아무 이름으로 바꿔준 후 다시 올바른 이름으로 바꿔준다.

```bash
git mv Login temp

git mv temp login
```

---

## 4. ESLint dependency cycle detected import/no-cycle Error

- 상황: 자식 컴포넌트에서 props 중 함수 prop의 타입을 지정해주려고 할 때 부모 컴포넌트에서 이미 지정해놓은 타입을 import 해오려고 했는데 에러가 발생했다.

- 원인: 자식 컴포넌트는 부모 컴포넌트에 import 되서 사용된다. 그런데 자식 컴포넌트에서 부모 컴포넌트의 요소를 import 하면 순환 에러가 발생한다. (자식 -> 부모 -> 자식)

- 해결: type.ts 모듈을 따로 만들어서 각 컴포넌트에서 import해서 사용하는 방식으로 해결 했다.

---

## 5. 리액트에서 select 사용하기

- `<input>`만 많이 사용하다가 `<select>`사용법이 햇갈려서 정리...(반성)

```tsx
<select value={value} onChange={handleChange}>
  {options.map(({ label, value }) => (
    <option key={value} value={value}>
      {label}
    </option>
  ))}
</select>
```

- 위 처럼 `input`태그에서 처럼 `onChange` 이벤트를 만들어주고 `value`에 `state`를 넣어주면된다.

---

## 6. jest에서 비어있는 요소 테스트하기

- `toBeEmptyDOMElement` matcher 사용
  ```ts
  expect(genderSelector.firstChild).toBeEmptyDOMElement();
  ```

---

## 7. tsx 파일에서 TypeScript 제네릭 사용시 에러 해결

- tsx 파일 내 arrow function 사용할 일이 있었는데 arrow function에 TypeScript의 제네릭을 사용하니 에러를 만나게됐다.

- 에러 내용: `JSX element 'T' has no corresponding closing tag.`

- 원인 추측: 다이아몬드 연산자가 .tsx에서 JSX구문으로 인식되기 때문에 에러가 발생한 것으로 보인다.

- 해결방법

  - 구글링으로 방법을 찾아보니 정상적인 방법은 없는 것 같았다. 그도 그럴것이 제네릭과 JSX는 겉보기에 똑같아서 개발자가 직접 알려주는 수 밖에는 없을 것 같다.
  - `<T,>(param : T) => x` 처럼 T 옆에 trailiing comma를 사용해서 `<`심볼이 jsx가 아니라는 확신을 컴파일러에게 주면 해결된다.
  - `const foo = <T extends {}>(x: T):T => x` 처럼 `{}`를 `extends`해줌으로써 jsx가 아니라는 확신을 준다.

- 한계점: 찾은 두가지 방법 모두 `Eslint`에서는 잘못된 코드라고 인식해서 `Eslint`에게도 해당 코드는 검사하지 않게 해줘야한다.

- Reference
  - [What is the syntax for Typescript arrow functions with generics?](https://stackoverflow.com/questions/32308370/what-is-the-syntax-for-typescript-arrow-functions-with-generics/45576880#45576880)
  - [TypeScript | Generic 제네릭 (feat. TypeScript 두 달차 후기)](https://velog.io/@edie_ko/TypeScript-Generic-%EC%A0%9C%EB%84%A4%EB%A6%AD-feat.-TypeScript-%EB%91%90-%EB%8B%AC%EC%B0%A8-%ED%9B%84%EA%B8%B0)

---

## 8. TypeScript에서 'vlueof'?

얼마전 특정 변수의 타입을 특정 객체의 key들로 해줄 일이 있었는데 그때 TypeScript의 `keyof`를 알게됐다. 그러면서 생긴 의문점이 'vlueof'는 없을까 하는 것이었다. 결론을 말하자면 'valueof'라는 키워드는 없다. 하지만 구글링 중 알게된 방법이 있어서 기록한다.

- VlueOf 타입 만들기

  ```typescript
  type ValueOf<T> = T[keyof T];
  ```

  ```typescript
  // 예시

  type Foo = { a: string; b: number };
  type ValueOfFoo = ValueOf<Foo>; // string | number
  ```

- 참고 자료: [Is there a `valueof` similar to `keyof` in TypeScript?](https://stackoverflow.com/questions/49285864/is-there-a-valueof-similar-to-keyof-in-typescript)

---

## 9. Jest, React Testing Library애서 Font Awesome 아이콘 테스트

- 상황: Font Awesome 아이콘을 사용하면서 아이콘이 화면에 잘 렌더링 되는지를 테스트 하기위해 `testid`를 이용했다. 그런데 이렇게 일일이 `testid`를 주는 것이 번거롭기도 했고 더 좋은 방법이 없을까 고민하던도중 함께 공부하는 ['건희 님'의 코드](https://github.com/xunxee/shopifyStore/pull/11/files/c8e6aa8dc3415b776a32ef4aea97069ae034fbc2#diff-8b030e2cdaff033951ed53cfe5cea2bae0de05f81583954fb8a0693bc88ed40a)에서 좋은 방법을 찾아서 이 방법을 정리한다.

- Font Awesome 컴포넌트에서 `title` props 이용하기

  - `title` props를 넘겨주게되면 title 태그를 렌더링해준다.
  - 브라우저에서 직접 보여지지는 않지만 어떤 아이콘인지 설명해준다는 점에서도 사용하는 것이 좋을 것 같다.
  - 위와 같이 SEO에서도 좋지만 테스트 코드에서 `getByText`를 이용해서 요소가 있는지 확인할 수 있다.

  ```tsx
  <FontAwesomeIcon
    title="birth-date-tooltip"
    icon={faCircleInfo}
    aria-hidden="true"
  />
  ```

  ```tsx
  expect(queryByText("birth-date-tooltip")).not.toBeNull();
  ```

---

## 10. TypeScript에서 input 태그의 types 적용

```tsx
import { HTMLInputTypeAttribute } from "react";

type InputBaseProps = {
  type?: HTMLInputTypeAttribute;
};
```

```typescript
type HTMLInputTypeAttribute = "number" | "search" | "button" | "time" | "image" | "text" | "checkbox" | "color" | "date" | "datetime-local" | "email" | "file" | "hidden" | "month" | "password" | "radio" | "range" | ... 5 more ... | (string & {})
```
