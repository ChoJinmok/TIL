# 2. Lifecycle Hooks

#### **`App.svelte`**

```html
<script lang="ts">
  import Something from "./Something.svelte";

  let toggle = false;

  function handleToggleButtonClick() {
    toggle = !toggle;
  }
</script>
```

```javascript
<button on:click={handleToggleButtonClick}>
  Toggle
</button>

{#if toggle}
  <Something />
{/if}
```

## onMount, onDestroy

#### **`Something.svelte`**

- 아래의 코드는 아직 컴포넌트가 화면에 렌더링 되기 전에 동작하기 때문에 h1태그를 못 찾는다.

```html
<script lang="ts">
  import { onMount, onDestroy } from "svelte";

  const h1 = document.querySelector("h1");
  console.log(h1.innerText);
</script>

<h1>Something..</h1>
```

- onMount에서 반환하는 함수는 컴포넌트가 연결 해제될 때 실행된다. (onDestroy와 동일하게 동작)
- 원래 onDestroy는 없었고 onMount에서 반환하는 함수로 로직을 처리해줬었다.
- 그런데 여러가지 이슈때문에 onDestroy가 생겼다.
- 실행 순서: onDestroy -> onMount에서 반환하는 함수
- 둘중 하나를 선택하면 된다.(두가지 모두 활용할 필요 없다.)
- 만약 onMount의 콜백함수가 async 함수인 경우 promise객체를 반환하게 되면서 아래 코드가 정상적으로 동작하지 않는다. -> 동기 함수일 때만 사용가능
- onDestroy의 callback 함수는 컴포넌트가 연결 해제되기 직전에 코드가 실행되기 때문에 콜백함수 안의 코드가 정상적으로 작동한다.

```html
<script lang="ts">
  import { onMount, onDestroy } from "svelte";

  onMount(() => {
    console.log("Mounted!");

    const h1 = document.querySelector("h1");
    console.log(h1.innerText);

    return () => {
      console.log("Destory in mount");
    };
  });

  onDestroy(() => {
    console.log("Destoryed!");

    const h1 = document.querySelector("h1");
    console.log(h1.innerText);
  });
</script>

<h1>Something..</h1>
```

## beforeUpdate, afterUpdate안에

- beforeUpdate(화면이 바뀓기전)와 afterUpdate(화면이 바뀐 후)는 반응성을 가지는 데이터가 할당을 통해서 갱신이 되면 실행된다. (컴포넌트가 연결될 때도 동작)
- 반응성을 가지는 로직이 beforeUpdate와 afterUpdate안에 있으면 무한 루프에 빠진다. -> 꼭 넣어야하는 상황이면 조건문으로 데이터가 갱신되지 않도록 한다.

```html
<script lang="ts">
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";

  let name = "Something..";
  let h1;

  function moreDot() {
    name += ".";
  }

  beforeUpdate(() => {
    console.log("Before update!");
    // 컴포넌트가 화면에 연결되서 화면에 출력되긴 전엔 h1은 undefined
    console.log(h1 && h1.innerText);
    // 마운트된 후 h1을 클릭하면 .이 추가 되기 전에 출력된다.
  });

  onMount(() => {
    console.log("Mounted!");
  });

  afterUpdate(() => {
    console.log("After update!");
    console.log(h1.innerText);
    // 점이 추가된 h1이 출력
  });

  onDestroy(() => {
    console.log("Destoryed!");
  });
</script>
```

```javascript
<h1 on:click={moreDot}>{name}</h1>
```
