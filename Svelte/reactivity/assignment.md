# 1. 할당(Assignment)

- Svelte는 항상 할당 연산자를 사용해야 반응성을 가지게된다.
- html 보간을 할수있는 데이터를 사용하려면 script 최상위 레벨에 변수를 정의해야한다.
- Svelte는 결국 할당연산자를 사용해서 반응성을 직접 만들어줘야한다. -> 반응성을 수동으로 만들어줘야한다.
  -> 가상돔을 사용하지 않기 때문 -> 불편한 대신 성능이 올라간다.

```html
<script lang="ts">
  let name = "Heropy";
  let fruits = ["Apple", "Banana", "Cherry"];
  const user = {
    name: "Heropy",
    depth: {
      a: "b",
    },
    numbers: [1, 2],
  };
  let { numbers } = user;
  const hello = "world";

  function assign() {
    name = "Jinmok";

    // script의 최상위 레벨이 아닌 함수 내부에서 변수 선언 -> 반응성 가지지 못함
    // const hello = 'world';

    // 일반적인 방법
    // const newFruits = [...fruits];
    // newFruits.push('Orange');
    // fruits = newFruits;

    // 다른환경에서는 보기 드문 방법이지만 Svelte에서는 많이 사용하는 방법
    // fruits.push('orange');
    // fruits = fruits;

    fruits = [...fruits, "Orange"];

    // 다음과 같이 변경해도 반응성을 가진다. -> 할당 표현에 user가 포함되면 반응성을 가진다.
    // user.name = 'Jinmok';
    // user.depth.a = 'c';

    // 아래 내용을 할당연산자가 포함돼있지 않기 때문에 반응성을 가지지 않을 거 같다.
    // 하지만 값이 업데이트 된다. -> tick을 사용하지 않으면 데이터를 갱신하더라도 기본 로직을 모두 처리하고 나서 화면을 갱신한다.
    // 만약 push 앞에 tick을 사용해서 값이 없데이트 되는 것을 기다리면 push로 인해 값이 변경되지 않는다.
    // cf) tick: 반응성을 가지는 데이터가 갱신되고 실제 화면이 갱신될 때까지 기다린다.
    // await tick();
    user.numbers.push(3);

    // 그냥 numbers는 갱신이 안된다. numbers에 할당 연산자를 사용하지 않았기 때문에
    numbers = numbers; // -> 갱신

    // 스벨트 홈페이지의 repl에서 output을 확인해보면 다음과 같이 컴파일되는 것을 알 수 있다.
    // $$invalidate(2, user.name = 'Jinmok', user);
    // $$invalidate(2, user.depth.a = 'c', user);
    // 첫번째 인자: 실행 순서
    // 두번째 인자: 할당 연산자를 통해서 값을 갱신하는 코드, 세번째 인자: 갱신되는 데이터
    // 세번째 인자의 현재 값으로 갱신된다. -> push된 내용을 모두 가지고 있다.
  }
</script>
```

```jsx
<button on:click={assign}>Assign!</button>

<h2>name: {name}</h2>
<h2>fruits: {fruits}</h2>
<h2>user name: {user.name}</h2>
<h2>user depth a: {user.depth.a}</h2>
<h2>user numbers: {user.numbers}</h2>
<h2>numbers: {numbers}</h2>
<h2>{hello}</h2>
```
