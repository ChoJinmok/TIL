# CSS Transition for only one type of transform?

> CSS에서 transition적용 시 transform 전체가 아닌 scale()과 같은 특정 값만 적용할 수 있을까를 고민하면서 생긴 의문점을 해결하는 과정에서 학습한 내용입니다.

<br />

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
