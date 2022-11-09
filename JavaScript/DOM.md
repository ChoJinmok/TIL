# DOM

> [udemy의 'The Web Developer 부트캠프 2022'](https://www.udemy.com/course/the-web-developer-bootcamp-2021-korea/)강의를 듣고 내용 정리

## 1. DOM 개요

- Document Object Model로 웹 페이지를 구성하는 JavaScript 객체들의 집합이다.
  - ex. h1 태그는 이를 모델링하는 JavaSciprt 객체가 있다.
  - 실제로 그 객체에 접근해서 h1을 변경하면 페이지에 변경 사항이 반영된다.
- DOM은 JS에서 웹페이지의 콘텐츠로 접근하는 창이자 통로이다. → JS로 HTML, CSS를 다룰 수 있다는 것! → 결국 페이지에 여러 다양한 효과를 줄 수 있게된다. (ex. 클릭이나 드래그에 반응하는 유용한 메서드와 프로퍼티가 포함되어 있다.)

## 2. Document Object

- 브라우저가 웹 페이지를 띄울 때 HTML과 CSS 정보를 받아들인 다음 요소와 스타일을 기반으로 JS 객체를 생성한다.
  ![makeDOM](./images/makeDOM.png)
- 객체마다 고유의 타입을 나타내는 특성을 갖게 된다.(href, src, innerText 등)
- 각 객체는 단독으로 존재하는 것이 아니라 모두 연결돼있다.(서로 관계를 가짐) → 트리구조
- Document: 트리 구조의 최상위에 있는 가장 중요한 요소이자 가장 중요한 객체
  - chrome에서 document를 보면 편의상 html로 보여준다 → 객체 구조로 보려면 console.dir사용
  - 모든 하위 객체들이 모여 document안에 표현되게 된다.(최상위 폴더와 비슷)
- document.all을 사용하면 HTMLALLCollection(유사 배열)이 반환되서 index로 접근 가능하다.

## 3. Selecting Method

- 텍스트, HTML, CSS를 직접 선택하는 것이 아니라 DOM의 Element(요소 노드)를 선택하는 것

### 3.1. getElementById

- document에 사용
- 문자열로 전달 -> 일치하는 ID를 가진 Element를 찾아낸다.
- 일치하는 요소가 없으면 null 반환
- 반환하는 값이 console창에는 HTML같이 출력되지만 실제로 HTML이 아니라 Element(요소 노드) (Chrome에서 편의상 HTML처럼 보여주는 것)

### 3.2. getElementsByTagName & getElementsByClassName

- Element's'에서 알 수 있듯 다수의 Element들을 반환한다.
- 일치하는 모든 요소를 찾아서 반환한다.
- 일치하는 요소가 없으면 null 반환
- HTMLCollection을 반환한다.
  - HTMLCollection은 Element들을 가지고 있다.
  - HTMLCollection은 유사배열이면서 이터러블 (반복문 사용가능)
  - 배열로 변환해서 사용하기 (live 객체이기 때문에 예상치 못한 에러가 발생할 수 있다.)

<br />

cf) [Element](https://developer.mozilla.org/ko/docs/Web/API/Element)

- JS의 객체
- 하나의 HTML 요소를 나타낸다 -> property들을 가지고 있다.

<br />

### 3.3. querySelector

- 비교적 더 최근에 추가된 메서드
- id, className, tagName, 타입, 속성, CSS 스타일 등 원하는 선택자를 querySelector에 전달해서 요소를 선택할 수 있다.
- 메서드에 CSS에 사용하는 선택자를 넣어준다.
  - 예시: `document.querySelector('img:nth-of-type(2)')`, `document.querySelector('a[title="Java"]')`
- 일치하는 요소 중 첫번째 요소만 반환한다.

### 3.4. querySelectorAll

- querySelector와 원리는 같지만 일치하는 모든 요소를 반환한다.
- NodeList형태로 반환
  - HTMLCollection과 같이 반복문 사용가능
  - HTMLCollection과 마찬가지로 배열로 변환 후 사용하기
