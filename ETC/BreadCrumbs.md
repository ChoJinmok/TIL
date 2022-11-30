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

## 5. CSS Transition for only one type of transform?

- 상황: CSS에서 transition적용 시 transform 전체가 아닌 scale()과 같은 특정 값만 적용할 수 있을까를 고민하면서 의문점이 생겼다.

```tsx
// 문제 발생 코드

const Container = styled.div({
  position: "absolute",
  top: "50%",
  left: 0,
  transform: "translateY(-50%)",
  transition: "transform 0.3s ease-in-out",

  "& hover": {
    transform: "scale(1.3)",
  },
});
```

- 결론은 transform의 특정 값에 대해서만 transition을 사용할 수 없다.

```tsx
// 아래와 같이 코드를 작성해줘도 어떤 이유에서인지 translateY의 계산이 원하던 대로 이루어지지 않았다.

const Container = styled.div({
  position: "absolute",
  top: "50%",
  left: 0,
  transform: "translateY(-50%)",
  transition: "transform 0.3s ease-in-out",

  "& hover": {
    transform: "scale(1.3) translateY(-50%)",
  },
});
```

- 하지만 div로 한번 더 감싸거나 클래스를 하나 더 만들어서 스타일을 적용하면 원하던 대로 스타일을 줄 수 있었다.

```tsx
// 아래 두 div로 요소를 감싸주면 원하던 대로 scale만 변화가 일어났다.

const PositionContainer = styled.div({
  position: "absolute",
  top: "50%",
  left: 0,
  transform: "translateY(-50%)",
});

const ScaleContainer = styled.div({
  transition: "transform 0.3s ease-in-out",

  "& hover": {
    transform: "scale(1.3)",
  },
});
```
