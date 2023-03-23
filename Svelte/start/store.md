# 6. 스토어

### 6.1. 스토어를 사용하지 않고 props로 관리

#### **`App.svelte`**

```html
<script lang="ts">
  import Parent from "./Parent.svelte";
  const name = "world";
</script>
```

```javascript
<h1>Hello {name}!</h1>
<Parent {name} />
```

#### **`Parent.svelte`**

```html
<script lang="ts">
  import Child from "./Child.svelte";

  export let name: string;
</script>
```

```javascript
<div>Parent</div>
<Child {name} />
```

#### **`Child.svelte`**

```html
<script lang="ts">
  export let name: string;
</script>
```

```javascript
<div>Child {name}</div>
```

- 위의 코드를 보면 Parent는 props를 전달해주는 역할만하는데 이는 비효율적 (다룰 필요도 없는 데이터를 중간에서 다뤄야한다.)

- Svelte는 Store로 전역상태를 관리해줄 수 있다.

<br />

### 6.2. 스토어를 사용해서 데이터 공유

- store객체: set(할당), subscribe(구독), update(수정) 기능이 있다. (`console(storeName)`)
- store 객체를 실제로 사용하려면 앞쪽에 `$`넣어줘야한다. (`console($storeName)`)
- set, update, subscribe 메서드를 사용해 구현하는 대신, 간단하게 $ 표시를 사용하는 것을 Auto-subscription(자동 구독)이라고 한다.

#### **`App.svelte`**

```html
<script lang="ts">
  import { storeName } from "./store";
  import Parent from "./Parent.svelte";

  const name = "world";
  // $ 가 붙어있으면 store 객체의 데이터로 보면 된다.
  $storeName = name;
  // 변수명을 정할 때 앞에 $를 붙이면 예약어 에러가 발생한다.
</script>
```

```javascript
<h1>Hello {name}!</h1>
<Parent {name} />
```

#### **`Parent.svelte`**

```html
<script lang="ts">
  import Child from "./Child.svelte";
</script>
```

```javascript
<div>Parent</div>
<Child />
```

#### **`Child.svelte`**

```html
<script lang="ts">
  import { storeName } from "./store";
</script>
```

```javascript
<div>Child {$storeName}</div>
```
