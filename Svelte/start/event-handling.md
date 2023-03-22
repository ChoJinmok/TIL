# 4. 이벤트 핸들링

### addEventListener

- `onMount`는 컴포넌트가 연결(mount)되면 콜백을 실행한다.
- 현재 컴포넌트가 준비되면 onMount로 전달한 콜백 함수가 동작한다.

```html
<script lang="ts">
  import { onMount } from "svelte";
  let isRed = false;

  function handleBoxClick() {
    isRed = !isRed;
  }

  onMount(() => {
    const box = document.querySelector(".box");
    box.addEventListener("click", handleBoxClick);
  });
</script>
```

```javascript
<div
  class="box w-80 h-40"
  class:bg-orange-400={!isRed}
  class:bg-red-600={isRed}
  on:click={handleBoxClick}
  on:keydown={handleBoxClick}
>
  Box!
</div>
```

- 하지만 위와 같이 addEventListener 방식으로 코드를 작성하면 번거로워진다.
  -> Svelte에서 제공해주는 api를 사용하는 것이 더 간단하다.

<br />

### on 디렉티브

- html태그에 `on` 디렉티브(키워드)를 작성해주면 된다.
- `on` 디렉티브에는 addEventListener에 붙일 수 있는 모든 이벤트들을 모두 작성할 수 있다.

```html
<script lang="ts">
  let name = "world";
  let isRed = false;
  let text = "";

  function handleBoxClick() {
    isRed = !isRed;
  }

  function handleBoxMouseEnter() {
    name = "enter";
  }

  function handleBoxMouseLeave() {
    name = "leave";
  }

  function handleInput({ target }: Event) {
    const { value } = target as HTMLInputElement;
    text = value;
  }

  function handleButtonClick() {
    text = "Jinmok";
  }
</script>
```

```javascript
<h1>Hello {name}!</h1>
<div
  class="box w-80 h-40"
  class:bg-orange-400={!isRed}
  class:bg-red-600={isRed}
  on:click={handleBoxClick}
  on:keydown={handleBoxClick}
  on:mouseenter={handleBoxMouseEnter}
  on:mouseleave={handleBoxMouseLeave}
>Box!</div>
<h1>{text}</h1>
<input
  type="text"
  value={text}
  on:input={handleInput}
/>
<input
  type="text"
  bind:value={text}
/>

// 위의 input 모두 양방향 바인딩을 해준 것이지만 `bind`사용하면 더 간단하게 양방향바인딩을 해줄 수 있다.
<button on:click={handleButtonClick}>Click</button>
```
