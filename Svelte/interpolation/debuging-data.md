# 3. 데이터 디버깅

- JS에서는 console을 활용해서 디버깅을 많이 해주는데 Svelte는 기본 디버깅 키워드(debug)를 제공한다.
- 보간에 작성돼있는 데이터가 변경될 때마다 콘솔에 내용을 출력해준다.
- 여러가지 데이터를 넣어줄 수 있다.
- 디버깅하려는 데이터에대한 추적 코드를 script쪽에 넣지않아도 html위치에 편하게 작성할 수 있다.
- 개발자 도구가 켜져있는 상태에서 데이터가 변경되면 프로세스를 일시정지 해준다. -> 불편할 수도 있음

```html
<script lang="ts">
  const name = "Jinmok";
  let index = 0;
</script>
```

```jsx
{@debug index, name}

<h1
  on:click={() => { index += 1; }}
  on:keydown={() => {}}>
  Hello {name}!
</h1>
```
