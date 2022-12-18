# jsx에서 mouse event는 key event로도 가능해야한다.

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)에서 `span` 태그에 `mouseover` 이벤트를 주다가 만난 [eslint에러](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/4abc751d87a8491219a9a3d2dacd80ea8adcb79b/docs/rules/mouse-events-have-key-events.md)를 해결하면서 알게된 점을 정리했다.

<br />

## 1. 'jsx-a11y/mouse-events-have-key-events' 에러

- `onmouseover/onmouseout`에는 `onfocus/onblur`가 수반된다.
- 마우스를 사용할 수 없는 신체 장애가 있는 사용자, AT 호환성 및 스크린리더 사용자에게 키보드 코딩은 중요하다.

## 2. 의문점

- `onmouseover/onmouseout`는 key 이벤트를 주라고 에러가 뜨는데 `onmouseenter/onmouseleave`에는 에러가 뜨지 않는다.
- 차이점이라면 이벤트 버블링을 통해서 이벤트가 전파된다는 것!

## 3. 결론

- mouse 이벤트에 key 이벤트를 주는 이유가 사용자의 초점에서 코딩을 해야한다는 것인데 `onmouseenter/onmouseleave`도 똑같이 적용된다.
- `onmouseenter/onmouseleave`에는 에러가 발생하지는 않지만 똑같이 key 이벤트를 주자!
