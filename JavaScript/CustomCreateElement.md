# Custom Create Element 만들기

> `프로그래머스 Dev-Matching: 웹 프론트엔드 개발자`를 준비하면서 자주 사용되는 로직을 함수로 미리 만들면서 공부한 내용을 정리했습니다. (ps. 코드숨 리액트과정 1주차)

## 1. 개기

- VanillaJS로 개발하면 당연하게도 DOM을 조작하는 일이 아~주 많다.
- 그 중 새로운 요소를 만들어서 DOM에 append(추가)해주는 일이 아주 번거롭게 느껴졌고 이를 추상화해서 사용하는게 좋겠다고 생각했다.

## 2. 기존 코드

```typescript
const data = [
  {
    name: "Jinmok",
    age: 32,
  },
  {
    name: "Vanilla",
    age: 28,
  },
];

const $container = document.getElementById("app");

const $peopleList = documnet.createElement("ul");

$peopleList.className = "peopleList";

data.forEach(({ name, age }) => {
  const $person = documnet.createElement("li");

  const textNode = documnet.createTextNode(
    `Hello, ${name}. You are ${age} years old?!`
  );

  $person.appendChild(textNode);

  $peopleList.appenChild($person);
});

$container.appendChild($peopleList);
```

- 리플로우, 리페인팅을 고려해서 코드를 짜다보면 위의 코드처럼 한두줄이더라도 더 길어지게 된다. (넣어줄 부모 태그가 없다면 fragment에 담아서 마지막에 fragment를 삽입한다.)

## 3. 추상화 코드

```typescript
function createElement(tagName, props, ...children) {
  const element = document.createElement(tagName);

  Object.entries(props || {}).forEach(([key, value]) => {
    if (key.startsWith("on")) {
      element[key.toLowerCase()] = value;
    }

    element[key] = value;
  });

  children.flat().forEach((child) => {
    if (child instanceof Node) {
      element.appendChild(child);
      return;
    }

    element.appendChild(document.createTextNode(child));
  });

  return element;
}
```

- 코드숨 1주차에 배웠던 코드를 응용했다.
  - 바벨은 JSX를 만나면 React.createElement를 이용해서 JavaScipt코드로 변환시킨다.
  - 결국은 React.createElement를 직접 구현한 것!
- **코드 설명**
  - 매개변수로 전달받은 tag를 생성한다.
  - property들을 객체로 전달받은 후 생성한 요소의 property로 추가해준다.
    - class는 className으로 넣어줘야한다. (JS의 class와 구분)
    - event property들은 카멜케이스를 사용하지 않고 모두 소문자로 넣어줘야한다. (리액트와 다른점)
    - 자식 요소로 배열이 들어오는 경우도 고려했고, Node의 instance인지로 텍스트와 요소를 구분해서 자식들을 추가해준다.

## 4. 변경 코드

```typescript
document.getElementById("app").appendChild(
  createElement(
    "ul",
    { className: "pepleList" },
    data.map(({ name, age }) =>
      createElement("li", null, `Hello, ${name}. You are ${age} years old`)
    )
  )
);
```
