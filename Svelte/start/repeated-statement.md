3. 반복문

```html
<script lang="ts">
  const name = "Fruits";
  let fruits = ["Apple", "Banana", "Cherry", "Orange", "Mango"];

  function deleteFruit() {
    fruits = fruits.slice(1);
  }
</script>
```

```javascript
<h1>Hello {name}!</h1>
<ul>
  <!-- Svelte의 반복문 -->
  {#each fruits as fruit}
    <li>{fruit}</li>
  {/each}
</ul>
<button on:click={deleteFruit}>
  Eat it!
</button>
```
