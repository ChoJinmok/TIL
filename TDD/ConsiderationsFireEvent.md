# Testing Library fireEvent에 대한 고려 사항

> Testing Library의 fireEvent로 mouse over 이벤트를 구현하던 도중 Testing Library의 좋은 글을 발견해서 요약 정리했다.

<br />

## 1. Interactions vs. events

[기본 원칙](https://testing-library.com/docs/guiding-principles/)에 따라 테스트는 사용자가 코드(component, 페이지 등)와 상호 작용하는 방식과 최대한 유사해야 한다. `fireEvent`는 사용자가 애플리케이션과 상호 작용하는 방식과 정확히 일치하진 않지만 대부분의 시나리오와 충분히 유사하다.

클릭 이벤트를 생성하고 지정된 DOM 노드에서 해당 이벤트를 전달하는 `fireEvent.click`은 고민해서 사용해야한다. `fireEvent.click`은 요소를 클릭할 때 발생하는 일을 테스트하는 대부분의 상황에서 제대로 작동하지만 사용자가 실제로 요소를 클릭하면 다음의 이벤트가 순서대로 발생한다.

- fireEvent.mouseOver(element)
- fireEvent.mouseMove(element)
- fireEvent.mouseDown(element)
- element.focus() (해당 요소가 포커스 가능한 경우)
- fireEvent.mouseUp(element)
- fireEvent.click(element)

그런 다음 해당 요소가 `label`의 자식인 경우 label된 form control로 포커스가 이동한다. 따라서 실제로 테스트하려는 것은 클릭 핸들러뿐이긴 하지만 단순히 `fireEvent.click`을 사용하면 그 과정에서 사용자가 실행하는 다른 잠재적 중요 이벤트들은 놓치게 된다.

다시 말하지만 대부분의 경우 이것은 테스트에 중요하지 않으며 단순히 `fireEvent.click`을 사용하는 것으로 충분하다.

## 2. 대안

component의 대화형 동작을 테스트할 때 신뢰도를 높일 수 있는 방법에 대해 알아보자. 다른 상호 작용의 경우 [`user-event`](https://testing-library.com/docs/user-event/intro/)를 사용하거나 실제 환경에서 component를 테스트하는 것을 고려할 수 있다.

### 2.1. Keydown

[현재 포커스가 있는 요소, body 요소 또는 document 요소에 keydown은 전달된다](https://w3c.github.io/uievents/#events-keyboard-event-order).
다음을 선호해야 한다.

```typescript
// 다음은 지양
// fireEvent.keyDown(getByText("click me"));

// 다음을 지향
getByText("click me").focus();
fireEvent.keyDown(document.activeElement || document.body);
```

또한 해당 요소가 키보드 이벤트를 수신할 수 있는지도 테스트한다.

### 2.2. Focus/Blur

요소에 초점이 맞춰지면 focus 이벤트가 전달되고 document의 활성 요소가 변경되며 이전에 초점이 맞춰진 요소가 blur 된다. 이 동작을 시뮬레이트하려면 `fireEvent`를 명령형 포커스로 바꾸면 된다.

```typescript
// 다음은 지양
// fireEvent.focus(getByText('focus me'));

// 다음을 지향
getByText("focus me").focus();
```

이 접근 방식의 좋은 side-effect는 요소가 포커스를 받을 수 없는 경우 발생한 포커스 이벤트에 대한 명령이 실패한다는 것이다. 이는 keydown 이벤트를 후속 조치하는 경우에 특히 중요하다.
