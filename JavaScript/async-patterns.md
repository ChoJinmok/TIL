# 자바스크립트 비동기 처리의 이해 및 사용 패턴

> 인프런의 [Svelte.js [Core API] 완벽 가이드](https://www.inflearn.com/course/%EC%8A%A4%EB%B2%A8%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)의 내용을 학습 후 정리 했습니다.

<br />

## 1. 동기

- 거의 대부분의 경우 다음과 같이 작성된 순서대로 동작한다. (동기 방식)

```javascript
function a() {
  console.log("a");
}

function b() {
  console.log("b");
}

a();
b();

// 콘솔 창
// a
// b
```

- 다음에서도 a함수를 먼저 실행하고 b함수를 실행했지만 결과는 b가 먼저 출려된다. -> 작성한 순서대로 출력되지 않았다.

```javascript
function a() {
  setTimeout(() => {
    console.log("a");
  });
}

function b() {
  console.log("b");
}

a();
b();

// 콘솔 창
// b
// 약 1초 뒤
// a
```

- 만약 a가 처리되는 것을 기다린 다음에 b가 처리되도록 하려면? -> 비동기 패턴이 필요

## 2. 비동기

### 2.1. 콜백 함수

- 콜백 함수는 어떠한 데이터를 바깥쪽에서 받아서 가지고 있는 상태
- 콜백 함수를 적절한 위치해서 실행시켜준다.
- 많은 경우에 함수의 인수로 또다른 함수가 들어갈 때 인수로 들어간 함수를 콜백함수라고 부른다.

```javascript
function a(callback) {
  setTimeout(() => {
    console.log("a");
    callback();
  });
}

function b() {
  console.log("b");
}

a(() => {
  b();
});

// 콘솔 창
// a
// b
```

#### **`콜백지옥`**

```javascript
function a(cb) {
  setTimeout(() => {
    console.log("a");
    cb();
  });
}

function b(cb) {
  setTimeout(() => {
    console.log("b");
    cb();
  });
}

function c(cb) {
  setTimeout(() => {
    console.log("c");
    cb();
  });
}

function d(cb) {
  setTimeout(() => {
    console.log("d");
    cb();
  });
}

a(() => {
  b(() => {
    c(() => {
      d(() => {
        console.log("Done!");
      });
    });
  });
});

// 콘솔 창
// a
// b
// c
// d
// Done!
```

- 각각의 순서를 보장하기 위해 어쩔 수 없이 위와 같이 코드를 작성했어야 했다.
- 순서는 보장됐지만 코드가 복잡해진다. (마치 개미지옥 같음)

### 2.2. Promise

```javascript
function a() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("a");
      resolve();
    });
  });
}

function b() {
  console.log("b");
}

// a함수를 실행하면 promise 객체가 봔환된다.
// promise 객체는 then이라는 메서드를 사용할 수 있다.
// then에 인자로 넣어준 콜백함수는 resolve 위치에서 실행된다.
a().then(() => {
  b();
});

// 콘솔 창
// a
// b
```

- `Promise` 생성자 함수는 무언가를 기다리는 것을 `약속`하는 인스턴스를 반환한다는 의미로 이해하면 된다.
- 여기서 약속은 `resolve`라는 매개변수가 어느 시점에 실행될 것인지에 대한 약속
- 비동기라는 개념에서 `Promise` 생성자와 매개변수 resolve로 어느 시점에 다음 내용의 실행을 보장할것인지 명시하는 것이 가장 중요하다.
