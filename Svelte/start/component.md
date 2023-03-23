# 5. 컴포넌트

### :: 컴포넌트 사용하지 않는 경우

```html
<script lang="ts">
  const fruits = ["Apple", "Banana", "Cherry", "Orange", "Mango"];
</script>
```

```javascript
<h2>Fruits</h2>
<ul>
  {#each fruits as fruit}
    <li>{fruit}</li>
  {/each}
</ul>
<h2>Fruits reverse</h2>
<ul>
  {#each [...fruits].reverse() as fruit}
    <li>{fruit}</li>
  {/each}
</ul>
<h2>Fruits slice -2</h2>
<ul>
  {#each fruits.slice(-2) as fruit}
    <li>{fruit}</li>
  {/each}
</ul>
```

- 위의 코드를 보면 똑같은 구조가 반복된다. ➡️ 반복 작업이 많아 져서 유지보수가 힘들어진다.

### :: 컴포넌트화

- props는 `export let`으로 받아 올 수 있다.
- export라서 내보내는 것 같지만 내보내는 것이 아니라 부모 컴포넌트와 연결할 수 있는 통로를 만드는 작업이라고 보면 좋겠다.

```html
<!-- Fruits.svelte -->

<script lang="ts">
  // Props
  export let fruits: string[];
  export let reverse = false;
  export let slice = "";

  function computeFruits() {
    if (reverse) return [...fruits].reverse();

    if (slice) {
      const slicePrams = slice.split(",").map((pram) => Number(pram));
      return [...fruits].slice(...slicePrams);
    }

    return fruits;
  }

  function makeTitle() {
    if (reverse) return "reverse";
    if (slice) return `slice ${slice}`;
    return "";
  }

  const title = `Fruits ${makeTitle()}`;
  const computedFruits = computeFruits();
</script>
```

```javascript
<h2>{title}</h2>
<ul>
  {#each computedFruits as fruit}
    <li>{fruit}</li>
  {/each}
</ul>
```

```html
<!-- App.svelte -->

<script lang="ts">
  import Fruits from "./Fruits.svelte";
  const fruits = ["Apple", "Banana", "Cherry", "Orange", "Mango"];
</script>
```

```javascript
<Fruits {fruits} />
<Fruits
  {fruits}
  reverse/>
<Fruits
  {fruits}
  slice="-2" />
<Fruits
  {fruits}
  slice="0, 3" />
```
