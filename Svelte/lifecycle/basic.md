# 1. 기본 개념

- Lifecycle은 컴포넌트가 연결되고 해제되는 등의 컴포넌트의 생명주기로 원하는 코드를 실행할 수 있도록 여러 기능을 제공한다.

#### **`App.svelte`**

```html
<script lang="ts">
  import Something from "./Something.svelte";

  let toggle = false;
</script>
```

```javascript
<button
  on:click={() => {
    toggle = !toggle;
  }}
>
  Toggle
</button>

// Toggle이 true가 되고 Something이 렌더링 될 때 "연결이 되었다(Mounted)"고 포현한다. -> `onMount()`
// Toggle이 false가 되고 Something이 사라졌을 때 "연결이 해제되었다(Destroyed)"고 포현한다. -> `onDestroy()`
{#if toggle}
  <Something />
{/if}
```

- toggle에 의해 반응성을 가짐 -> 반응성을 가지는 데이터를 다룰 때
- "화면이 바뀌기 전(Before update)" -> `beforeUpdate()` -> 화면이 바뀌기 전 상태를 제어할 수 있다.
- "화면이 바뀐 후(After update)" -> `afterUpdate()` -> 화면이 바뀌고 난 상태를 제어할 수 있다.

- [컴포넌트를 연결하면, (최초)] -> beforeUpdate --(DOM과 데이터를 동기화(렌더링))--> onMount -> afterUpdate | onDestroy(연결시엔 실행 X)

- [컴포넌트를 연결 해제하면] -> onDestroy (다른 Lifecycle 생략)
- 완벽하게 떨어지기 전 = 분리가 되기 전에 = 연결이 실제 해제되기 직전에 onDestroy가 실행된다.

- [컴포넌트의 데이터를 갱신하면] -> beforeUpdate --(DOM과 데이터를 동기화(렌더링))--> afterUpdate (onMount 생략)

- 각 Lifecycle을 Lifecycle Hook(훅, 후크)라고도 부른다. (E.g. onMount Hook, beforeUpdate Hook)

#### **`Something.svelte`**

```html
<h1>Something</h1>
```
