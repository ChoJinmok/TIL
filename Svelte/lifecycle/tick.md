# 4. tick

- tick: 데이터가 갱신되고 나서 화면이 바뀌는 반응성을 가질 때까지 잠시 기다릴 수 있게 해줌
- tick함수는 프로미스 객체를 반환한다. -> 비동기로 실행해야한다.
- Svelte는 전체 로직의 최적화를 위해 기본적인 로직들을 모두 실행하고 나면 화면을 갱신하도록 설계되었다.
- 화면이 갱신된 이후에 처리해야할 로직이 있다면 tick을 사용하여 화면이 갱신될 때까지 기다린다.
- onMount, onDestroy, beforeUpdate, afterUpdate는 언제 실행되는지가 정해져 있지만 tick은 실행 위치를 내가 정할 수 있다.

```html
<script lang="ts">
  import { tick } from "svelte";

  let name = "world";

  async function handleTitleClick() {
    name = "Jinmok";

    await tick();

    const h1 = document.querySelector("h1");

    // 갱신된 Jinmok이 콘솔창에 입력될 거 같지만 world가 입력된다.
    console.log(h1.innerText);
  }
</script>
```

```jsx
<h1 on:click={handleTitleClick} on:keydown={handleTitleClick}>
  Hello {name}!
</h1>
```
