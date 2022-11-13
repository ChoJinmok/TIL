# DOM Events

> [udemy의 'The Web Developer 부트캠프 2022'](https://www.udemy.com/course/the-web-developer-bootcamp-2021-korea/)강의를 듣고 내용 정리

<br />

### Event: 사용자가 특정 행동을 할 때 반응하는 행동

<br />

## Inline Event

```html
<button onclick="alert('you clicked me!')">Click Me!</button>
```

- Inline으로 Event를 넣어주면 단점이 많다.

  - html안에 JS를 바로 사용하는 방법이라 마크업이 길어짐 (따옴표도 신경써줘야한다)
  - 만약 JS 코드가 길어지면 세미콜론으로 구분해줘야하는데 가독성이 떨어진다.
  - 똑같은 기능을 하는 버튼을 또 만들고 싶으면 일일이 하드 코딩해줘야한다.
  - JS와 관심사 분리를 하기가 쉽지않아진다. (React의 경우는 다르다.)

- JS에서 Element의 속성을 살펴보면 내부 슬롯중 [[FunctionLocation]]의 값이 html파일인 걸 볼 수 있다.

## Element의 property

```tsx
const button = document.queySelector("button");

button.onclick = function () {
  console.log("YOU CLiCKED ME!");
  console.log("I HOPE IT WORKED!!");
};
```

## addEventListener

```tsx
const button = document.queySelector("button");

button.addEventListener("click", () => {
  alert("You Clicked me!!");
});
```

- HTML의 attribute, DOM Element의 property를 이용하는 방법보다 addEventListener가 장점이 더 많다.
  - **핸들러 축적가능**  
    이벤트 하나에 여러 핸들러를 추가하고 싶은경우 여러번 property에 핸들러를 담아주면 마지막값으로 덮어씌워진다. (객체의 property에 두번 넣어주는 경우라 어찌보면 당연)
    ```tsx
    button.onclick = handler1;
    button.onclick = handler2; // 최종적으로 2번 핸들러만 적용된다.
    ```
    ```tsx
    // addEventListener는 중첩으로 넣어도 핸들러가 모두 추가된다.
    button.addEventListener("click", handler1);
    button.addEventListener("click", handler2);
    ```
    ex. drag&drop을 구현할 경우 여러 함수를 하나의 함수로 만들어서 property에 추가하는 방식으로 구현하는 것이 쉽지 않기 때문에 addEventListener의 축적기능이 유용하다.
  - **옵션**으로 넣어줄수 있는 매개변수가 많이 존재한다. ⇒ 더 유연하게 사용가능!
    ```tsx
    // once를 넣어주면 처음에 한번만 실행되고 eventListener가 제거된다.
    button.addEventListener("click", handler, { once: true });
    ```
  - 이벤트와 이벤트 리스너가 많이 발생하는 복잡한 앱인 경우 removeEventLister로 잘 관리해줘야한다.
- Event & this: addEventListener의 이벤트 핸들러에서 this는 addEventListener 메서드를 부른 요소이다.

## 이벤트 객체

- 이벤트에 대한 정보를 담고 있는 객체
- 이벤트 객체는 보통 이벤트 핸들러에 자동으로 전달된다. → 이벤트 핸들러의 첫번째 인자로 전달된다.
  ex. 클릭 이벤트에서 클릭된 좌표를 알 수 있고, 키보드 이벤트에서 어떤 키가 눌렸는지도 알 수 있다.

## 키보드 이벤트에서 key, code

- **key**: 실제로 입력된 code가 들어간다. (space → 공백)
- **code**: 대응하는 key의 이름이 들어간다. (왼쪽 shift → ShiftLeft)
- 만약 설정 언어를 바꾸면 key는 변경된 언어로 들어가지지만 code는 언어를 바꾸어도 항상 같은 값이 들어온다.  
  => 키보드 이벤트의 글자를 받아와야하는 상황이면 key, 무슨 자판이 눌렸는지 알아야하는 상황이면 code 사용

```tsx
// 보통 게임에서는 window에 이벤트 리스너를 추가한다.

window.addEventListener('keydown', function (event) {
	console.log(event.code);
}
```

## Form Events & preventDefault

- form은 기본적으로 이벤트 핸들러가 동작한 후 action에 데이터를 전송 요청한다. (요청하면서 페이지 이동)
- 이 과정이 빠르게 지나가서 내가 작업하려고 했던 것들이 중단되는 느낌이지만 실제로는 내가 의도한 작업이 일어나고 순식간에 페이지가 이동된다.
- 이를 막고 싶다면 JS로 브라우저에게 페이지의 주소를 이동시키는 기본 동작을 막는걸 알려줘야한다. → 이벤트 객체의 preventDefault메서드 사용!
- preventDefault는 이벤트 결과로 일어날 기본 동작을 방지한다.
- form안의 input의 값들을 일일이 가지고 오는건 번거롭다: form 요소를 불러와서 요소의 `$form.elements` property를 이용해서 값을 불러오는 것이 편하다. (input에 name을 지정해 놓으면 elements에 key값이 name인 곳에 input 요소가 들어가있다.)
