# jsx의 div에 이벤트 주기
>[jsx-a11y/click-events-have-key-events](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/2b133ecc0ba1b882ca06f0813b1afcdf64195757/docs/rules/click-events-have-key-events.md)  
>[jsx-a11y/no-static-element-interactions](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/2b133ecc0ba1b882ca06f0813b1afcdf64195757/docs/rules/no-static-element-interactions.md)  
>[jsx-a11y/interactive-supports-focus](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/2b133ecc0ba1b882ca06f0813b1afcdf64195757/docs/rules/interactive-supports-focus.md)  
>Eslint에서 위의 error을 만나고 학습하게 됐습니다.

### 1. div에 click이벤트를 주려면 key이벤트가 필요하다.
- Eslint jsx-a11y에서는 div에 onClick을 강제하려면 onKeyUp, onKeyDown, onKeyPress 중 최고한 하나가 필요하다고 한다.
- 이유는 키보드 사용은 가능하지만 마우스 사용에는 제한이 있을 수 있는 사용자, AT 호환성 및 스크린 리더 사용자에게는 key 이벤트가 중요하다는 것이다.

### 2. span, div와 같은 static html 요소에 의미 더하기
- static 요소에 역할을 더해줘서 사용자에게 편의를 제공해줄 수 있다.
- 역할을 더해줄 뿐이지 자동으로 기능은 추가되지 않는다.
- role attribute를 추가해준다.

### 3. 역할과 기능을 추가해주면 포커스가 가능해야한다.
- 역할만 기능만 추가해주는 것이 아니라 접근을 가능하게 해줘야한다.
- tabIndex로 탭키로 접근가능하게 해준다.

### 완성 코드
```jsx
<div
    className="moviePoster"
    role="link"
    onClick={handleClick({ id, title, posterPath })}
    onKeyDown={handleKeyDown({ id, title, posterPath })}
    tabIndex={0}
>
```