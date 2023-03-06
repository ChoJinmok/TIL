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

#### **`then 체인`**

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
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("b");
      resolve();
    });
  });
}

function c() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("c");
      resolve();
    });
  });
}

function d() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("d");
      resolve();
    });
  });
}

a().then(() => {
  b().then(() => {
    c().then(() => {
      d();
    });
  });
});

// 콘솔 창
// a
// b
// c
// d
```

- 위와 같은 코드는 정상적으로 동작하지만 잘못된 방법이다. (콜백 지옥과 유사하다.)
- then 메서드의 콜백함수가 promise 객체를 return한다면 then 메서드가 끝나는 시점에 다시 then 메서드를 사용할 수 있다.

```javascript
a().then(() => {
  return b().then();
});

a()
  .then(() => {
    return b();
  })
  .then();

a()
  .then(() => b())
  .then();
```

```javascript
a()
  .then(() => b())
  .then(() => c())
  .then(() => d());

// 콘솔 창
// a
// b
// c
// d
```

- then 메서드를 체인형태로 이어서 사용하려면 then 메서드의 콜백 함수가 promise 객체를 반환해야한다.

#### **`Async Await`**

- `Promise`생성자가 생성한 promise 객체앞에 `await`키워드를 작성할 수 있다. (then 메서드와 조건이 같다.)

```javascript
async function asyncFn() {
  await a();
  await b();
  await c();
  await d();
}

asyncFn();

// 콘솔 창
// a
// b
// c
// d
```

- `await`는 `async` function 내부에서 사용할 수 있고 순서를 보장해준다.

#### **`reject`**

- promise가 코드의 실행 위치를 보장해주는 약속을 지키지 못하는 상황이 되면 reject가 실행된다.
- reject를 활용해서 약속을 지키지 못한 이유와 같은 메세지를 바깥쪽으로 넘겨줄 수 있다.
- reject가 실행될 때 catch메서드로 넘겨준 콜백함수가 동작한다.
- 로직에 문제가 있을 떄 reject가 실행되도록 Promise 생성자를 설계하면 된다.

```javascript
function a() {
  return new Promise((resolve, reject) => {
    if (isError) {
      reject("Sorry...");
      // return;
      // 아래쪽 코드가 실행되는 것을 방지 해주고 싶은 경우 return
    }

    setTimeout(() => {
      console.log("a");
      resolve();
    });
  });
}

a()
  .then(() => {
    console.log("b");
  })
  .catch((err) => {
    console.log(err);
  });

// isError = false
// a
// b

// isError = true (b는 출력되지 않는다.)
// Sorry...
// a
```

```javascript
async function asyncFn() {
  try {
    await a();
    console.log("b");
  } catch (err) {
    console.log(err);
  }
}

asyncFn();
```

#### **`finally`**

```javascript
function a() {
  return new Promise((resolve, reject) => {
    if (isError) {
      reject("Sorry...");
      return;
    }

    setTimeout(() => {
      console.log("a");
      resolve();
    });
  });
}

a()
  .then(() => {
    console.log("b");
  })
  .catch((err) => {
    console.log(err);
  })
  .finally(() => {
    console.log("Done!");
  });

// isError = true
// Sorry...
// Done!

// isError = false
// a
// b
// Done!
```

- 기존의 코드는 `reject`가 실행되면 `catch`가 실행(`then`은 실행 X), `resolve`가 실행되면 `then`이 실행(`catch`는 실행 X)됐다. 즉, `then`과 `catch`는 서로 상충되는 개념이다. `finally`는 `reject`, `resolve` 둘중 어떤 것이 실행되든 마지막에 한번 실행된다.

- 실무에서는 비동기 통신 시에 로딩을 완료되면 정상처리든 에러처리든 상관없이 로딩을 멈추는 기능을 구현할 때 `finally` 사용

- async/await 구문은 아래와 같이 작성 가능

```javascript
async function asyncFn() {
  try {
    await a();
    console.log("b");
  } catch (err) {
    console.log(err);
  } finally {
    console.log("Done!");
  }
}

asyncFn();
```

> #### **`resolve, reject의 인자`**
>
> ```javascript
> function a() {
>   return new Promise((resolve, reject) => {
>     if (isError) {
>       // reject의 인자는 꼭 문자열일 필요는 없다. 에러객체도 가능!
>       // reject("Sorry...");
>       reject(new Error("Sorry..."));
>       return;
>     }
>
>     setTimeout(() => {
>       console.log("a");
>       // resolve에도 인자(데이터 타입에 제한은 없음)를 넣어줄 수 있다.
>       resolve("Jinmok");
>     });
>   });
> }
>
> async function asyncFn() {
>   try {
>     // resolve의 인자로 넣어준 값을 반환한다.
>     // 변수명 res는 response(응답) 혹은 result(결과)의 약어로 사용
>     // 단순하게, 정상적인 비동기 처리의 '응답' 또는 '결과'
>     const res = await a();
>     console.log(res);
>     console.log("b");
>   } catch (err) {
>     console.log(err.message);
>   } finally {
>     console.log("Done!");
>   }
> }
>
> asyncFn();
> ```
>
> - 비동기 처리에서 문제가 발생하는 경우 대부분 에러 객체가 반환된다.
> - 에러 객체(`new Error()`의 인스턴스)는 `message` 속성을 가지고 있다.
>
> ```javascript
> a()
>   .then((res) => {
>     console.log(res);
>     console.log("b");
>   })
>   .catch((err) => {
>     console.log(err.message);
>   })
>   .finally(() => {
>     console.log("Done!");
>   });
> ```

### 2.3. 비동기 상태

- 대기(pending): 비동기 함수가 실행되고 나서 '이행'하거나 '거부'되지 않은 초기 상태.
- 이행(fulfilled): 연산이 성공적으로 완료됨. -> resolve, then, try에 실행되는 부분
- 거부(rejected): 연산이 실패함. -> reject, catch에서 실행되는 부분
