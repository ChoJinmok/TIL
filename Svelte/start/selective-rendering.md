# 1. 선언적 렌더링

- 반응성: 데이터가 바뀌면 화면도 갱신되는 것
- 데이터에 새로운 값이 할당되면 그 때 반응성을 가진다고 할 수 있다.
- 함수를 선언만하고 호출을 해주지 않는다면 반응성을 가진다고 할 수 없다.
  -> Svelte가 반응성이 없다고 판단해서 컴파일에 반영되지 않는다.

```html
<script lang="ts">
  let name = "world";
  let age = 85;

  // setTimeout(() => {
  //   name = 'Jinmok';
  //   age = 31;
  // }, 3000);

  function assign() {
    name = "Jinmok";
    age = 31;
  }
</script>
```

```javascript
<h1 class="text-red-600">Hello {name}!</h1>

// age < 85 ? 'text-blue' : '' => text-blue={age < 85}
<h2 class:text-blue-600={age < 85}>{age}</h2>
<img alt="{name}" src="" />

// 단방향 바인딩
<input type="text" value="{name}" />

// 양방향 바인딩
<input type="text" bind:value="{name}" />

<button on:click="{assign}">Assign</button>
```

```html
<!-- 기본적으로 svelte의 style은 해당 컴포넌트내에서만 유효함 -->
<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
```
