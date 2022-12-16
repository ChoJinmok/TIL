# aria-label, aria-labelledby, aria-describedby ?

> [Eslint 에러를 해결하는 과정](https://github.com/ChoJinmok/TIL/blob/main/React.js/jsxControlLabel.md)에서 aria 관련 attribute를 구글링하던 도중 [찾은 블로그 글](https://benmyers.dev/blog/aria-labels-and-descriptions/)을 공부하는 김에 해석하면서 요약 정리했다.

 <br />

## 1. Introduction

- **ARIA**는 웹 페이지가 보조 기술에 노출되는 방식을 조정하도록 설계된 HTML 속성 집합이다.

- 그 중 유사한 세 가지 ARIA 속성인 aria-label, aria-labelledby 및 aria-describedby에 대해 설명해보겠다.

## 2. Names and Descriptions

브라우저는 **접근성 트리**라고 하는 DOM의 대체 버전을 패키징하여 보조 기술에 노출한다. 이 트리는 요소의 기능을 설명하는 속성들로 구성되어있다. 이 속성 중 접근 가능한 이름과 설명이 있다.

**액세스 가능한 이름**은 필수 필드이다. 요소가 보조 기술에 의해 노출되는 데 중요한 역할을 한다. 이름은 페이지를 사용하는 다른 사람에게 요소를 설명하는 방법이다. 시맨틱 HTML 요소에는 보통 이름을 설정하는 기본 방법이 있다. (e.g. `image - alt text`, `button, link - text content`, `form field - <label>`)

이름은 보조 기술을 통해 요소와 상호 작용하는 데도 중요하다. 음성 컨트롤 사용자는 버튼과 같은 컨트롤 이름을 큰 소리로 말하여 컨트롤과 상호 작용할 수 있다. 또한 VoiceOver의 Rotor와 같은 일부 화면 판독기 탐색 모드에서는 사용자가 요소의 역할과 이름만 사용하여 페이지를 훑어볼 수 있다.

반면에 **액세스 가능한 설명**은 선택 사항이며 주어진 요소에 대한 추가 정보를 나타낸다. 스크린 리더 및 기타 보조 기술은 연속 읽기와 같은 일부 탐색 모드에서 설명이 불필요한 혼란을 유발할 수 있는 설명을 건너뛰도록 선택할 수 있다.

## 3. aria-label

**aria-label은 요소의 이름을 재정의하여 지정한 텍스트로 바꾼다**.

```html
<button aria-label="Close">×</button>
```

기본적으로 버튼의 이름은 "×"였다. 그러나 ×는 곱셈 기호를 의미하며 스크린리더가 이를 알려준다. 즉, 이 버튼은 닫기 버튼일 수 있지만 보조 기술에 의존하는 사용자에게는 의도와 다르게 의미가 전달된다. 이를 해결하기 위해 버튼에 `aria-label="Close"`를 넣었다. 버튼의 이름은 이제 "닫기"이며 훨씬 더 직관적이다.

화면 판독기 사용자를 위한 알림 및 발음을 맞춤화하기 위해 모든 곳에서 aria-label을 사용하고 싶을 수 있지만 모든 ARIA와 마찬가지로 신중해야 한다. 우선 aria-label은 대화형 요소에 대해 잘 지원되지만 **정적 텍스트 요소에 대해서는 잘 지원되지 않는다**. 또 레이블 재정의는 사용자가 페이지를 탐색하는 방법에 대한 잘못된 가정에 의존하는 경우가 많다. 마지막으로, aria-label을 자주 찾는다면 의미론적 마크업을 재고하거나 모든 사람이 액세스할 수 있는 더 많은 시각적 레이블을 포함하도록 디자인을 재정비해야 한다는 신호일 수 있다.

## 4. aria-labelledby

**aria-labelledby는 다른 요소의 내용으로 바꿔서 요소의 이름을 재정의한다**. aria-labelledby는 다른 요소의 id로 설정된다. aria-labelledby로 연결하면 콘텐츠를 한 곳에서만 업데이트하면 액세스 가능한 이름이 자동으로 업데이트되므로 반복적으로 이름을 설정할 필요가 없다.

aria-labelledby의 편리한 기능 중 하나는 섹션에 레이블을 지정하는 것이다. 섹션에 레이블이 지정되면 스크린 리더 사용자는 관심 있는 페이지의 섹션으로 건너뛰는 데 사용하여 목차처럼 섹션을 훑어볼 수 있다. 그러나 일반적으로는 제목 요소를 사용한다.

```html
<section aria-labelledby="intro-heading">
  <h2 id="intro-heading">Introduction</h2>
  <p>…</p>
</section>
```

또한 aria-labelledby와 aria-label이 모두 제공되는 경우 aria-labelledby는 aria-label을 대체한다.

## 5. aria-describedby

**aria-describedby는 요소의 설명을 다른 요소의 내용으로 설정한다**. aria-labelledby와 마찬가지로 aria-describedby는 ID를 사용한다.

예를 들어 aria-describedby를 사용하여 입력의 예상 형식에 대한 추가 세부 정보를 제공하는 요소와 입력을 연결할 수 있다.

```html
<form>
  <label for="username">Username</label>
  <input id="username" type="text" aria-describedby="format" />
  <p id="format">Username may contain alphanumeric characters.</p>
</form>
```

위의 예시에서 보조 기술은 입력 필드를 "username"이라고 부르지만 사용자가 실제로 필드를 탐색할 때 이름과 예상 형식을 모두 알려준다.

설명은 보충적이므로 모든 탐색 모드에서 사용자에게 표시되지 않을 수 있다. 이는 혼란을 줄여준다. 예를 들어 대부분의 스크린 리더 사용자는 양식을 작성하기 전체 양식을 훑어보는데 작성하기 전에는 설명이 필요하지 않다. 그러나 이는 설명이 제공되지 않을 수 있다는 것을 의미한다.

## 6. Providing Multiple IDs

필요한 경우 aria-labelledby 및 aria-describedby에 여러 ID를 전달할 수 있다. 여러 요소의 이름이나 설명을 조합하면 브라우저는 각 요소의 내용을 하나의 큰 문자열로 연결한다.

```html
<form>
  <label for="username">Username</label>
  <input id="username" type="text" aria-describedby="length format" />
  <p id="length">Username must be 6 to 15 characters.</p>
  <p id="format">Username may contain alphanumeric characters.</p>
</form>
```

입력의 전체 설명은 이제 "Username must be 6 to 15 characters. Username may contain alphanumeric characters."라고 표시된다.

## 7. Hidden Labels and Descriptions

aria-labelledby 및 aria-describedby는 hidden 또는 aria-hidden과 같은 몇 가지 다른 속성과 상호 작용한다. 호환되는 요소에 hidden 또는 aria-hidden="true"를 설정하면 해당 요소는 보조 기술에 노출되지 않으므로 스크린리더가 이를 알리지 않는다.

aria-labelledby 또는 aria-describedby에서 숨겨진 요소의 id를 사용하면 숨겨진 요소는 숨겨진 상태로 유지되지만 해당 콘텐츠는 다른 요소의 이름이나 설명을 채운다. 이러한 경우에는 aria-label을 사용하는 것이 더 나울 수 있다.

현재로서는 aria-description 속성이 아직 없지만 이 속성이 제공될 예정이다. 하지만 이 기능이 제공되기 전까진 요소의 설명을 설정하는 유일한 방법은 페이지에 다른 DOM 노드를 만들어야 한다는 것이다. 기본적으로 이것은 보조 기술이 레이블 지정/설명 요소가 독립적으로 노출될 수 있다는 것을 의미한다. 오해의 소지가 있는 혼란을 최소화하기 위해 설명에 aria-hidden를 배치할 수 있다.

```html
<table>
  <thead>
    <tr>
      <th role="columnheader" scope="col" aria-sort="none">
        <button aria-describedby="sort-description">
          <svg><!-- some sort icon --></svg>
          Name
        </button>
      </th>
      <th>…</th>
    </tr>
  </thead>
  <tbody>
    …
  </tbody>
</table>
<p id="sort-description" hidden>Sort this table alphabetically by name.</p>
```

정렬 아이콘은 시각 장애인 사용자에게 기능에 대한 어떠한 단서도 주지 못한다. 이를 보완하기 위해 테이블 ​​외부의 `<p>`를 가리키는 aria-describedby를 사용하여 버튼에 설명을 제공한다. 그러나 사용자가 `<p>` 자체를 탐색하는 것은 원하지 않으므로 여기에 hidden을 적용한다.

## 8. TL;DR

aria-label, aria-labelledby 및 aria-describedby는 모두 보조 기술에 노출될 때 요소를 명확하게 하는 데 사용할 수 있다.

- aria-label은 지정한 내용으로 요소의 이름을 재정의한다.

- aria-labelledby는 요소의 이름을 페이지에 있는 다른 노드의 콘텐츠로 바꾼다. 이미 보이는 레이블이 있을 때 이것을 사용한다.

- aria-describedby는 요소의 설명을 페이지의 다른 노드 내용으로 설정한다. 중요하지 않은 추가 정보에 적합한다.
