# CSS Opacity, visibility, display

> 현재 진행중인 프로젝트 [이케해여](https://github.com/ChoJinmok/ikehaeyeo)에서 스타일링을 적용하던 도중 Opacity, visibility, display의 차이에 대해 궁금해서 학습하고 정리했다.

<br />

## 1. 차이점

| 속성                  | collapse | 이벤트 | tab-order |
| --------------------- | -------- | ------ | --------- |
| `opacity: 0;`         | X        | O      | O         |
| `visibility: hidden;` | X        | X      | X         |
| `display: none;`      | O        | X      | X         |

- collapse: 영역이 무너지는가를 의미한다. 영역이 무너지면 화면에서 아무 공간도 차지하지 않게 된다.
- 이벤트: 클릭과 같은 이벤트가 동작하는지를 의미 -> 이벤트가 가능하다는 것은 뒤쪽에 요소가 가려져 있는 경우 클릭이 불가능하다. 반대로 이벤트가 동작하지 않는다는 것은 뒤쪽의 요소가 클릭이 가능하다는 것을 의미한다.
- tab-order: tab 키로 focusing이 가능한지를 의미

- `visibility: hidden`은 `opacity: 0`와 `pointer-events: none`의 조합처럼 동작한다.
- `display`와 `visibility`는 `animation`이 일어나지 않는다. `opacity`를 함께 사용해서 `animation`을 만든다. (사라진 때는 `animation`이 적용되지 않는다.)
- `transition` 전이 속성으로 fade in, fade out 효과는 `opacity`, `visibility`속성에 반응한다.

## 2. 실제 적용

- 만약 애니메이션적인 표현이 중요하지 않고 아예 화면에서 사라지는 것이 목표라면 `opacity`나 `visibility`속성보다는 `display: none;` 속성을 사용하는 것이 좋을 것 같다. 이유는 해당 요소가 DOM 트리까지는 포함되지만 **렌더 트리**에서 제외되기 때문에 자원 낭비를 조금이나마 줄일 수 있다.

- 하지만 위에서 설명대로 `display: none;`를 사용하면 해당 요소는 DOM 트리에 포함되게돼서 리액트에서 컴포넌트를 생성하는 계산 과정을 모두 거치게 돼서 리렌더링에 따른 자원 소모는 피할 수 없다.

- 최적화 측면에서는 **조건부 렌더링**을 통해서 계산 과정에서 완전 제외 시켜주는 것이 좋다.

- 만약 애니메이션 효과를 주면서 SEO를 최적화 시켜주고 싶다면 `visibility`속성을 활용해야한다. `opacity: 0`은 SEO에서 제외되지 않고 `visibility: hidden`은 SEO에서 제외시킨다.

- 애니메이션 효과를 주면서 뒤쪽 요소의 클릭도 제어하고 싶다면 `opacity`, `visibility`속성을 함께 사용해준다.
