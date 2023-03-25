# 2. onMount, onDestroy

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
