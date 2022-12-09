# control(interactive element)는 label과 연결되어야 한다.

> eslint(jsx-a11y)에서 [control-has-associated-label](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/main/docs/rules/control-has-associated-label.md) 에러를 만나고 해결하는 과정에서 알게된 것들을 정리했다.

<br />

## 1. 상황

```tsx
<select id={id} name={name} value={value} onChange={handleChange}>
  // <option value="" /> eslint 에러 발생
  {genderOptions.map(({ value: genderValue, text }) => (
    <option key={genderValue} value={genderValue}>
      {text}
    </option>
  ))}
</select>
```

텍스트 요소가 없는 `option`을 넣어주니 에러가 발생했다.

## 2. 원인

- [control-has-associated-label](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/main/docs/rules/control-has-associated-label.md) 에러를 확인해보니 말그대로 control element는 요소를 설명해줄 수 있는 label을 가지고 있어야하는데 `option`에 요소를 설명해줄 수 있는 label이 없기 때문에 에러가 발생했다.

- label을 붙여주는 방법
  - 요소 내부에 텍스트 콘텐츠를 제공
  - 요소에 [`aria-label`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-label) 속성을 사용
  - 요소에 [`aria-labelledby`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-labelledby) 속성을 사용하고 액세스 가능한 레이블이 있는 요소를 IDREF 값으로 가리킨다.
  - `img` 태그는 `alt` 속성을 사용하여 이미지에 대한 텍스트 설명을 제공할 수 있다.

## 3. 해결

```tsx
<select id={id} name={name} value={value} onChange={handleChange}>
  // aria-label을 사용해서 해당 option을 설명해준다.
  <option aria-label="gender none" value="" />
  {genderOptions.map(({ value: genderValue, text }) => (
    <option key={genderValue} value={genderValue}>
      {text}
    </option>
  ))}
</select>
```
