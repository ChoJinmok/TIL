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
