# JavaScript에서 Key event 처리
>리액트를 사용하고는 key event를 사용할 일이 많지 않았는데 오랜만에 이벤트를 처리해주니 햇갈려서 정리하게 됐습니다.

### 1. keyCode는 Deprecated...
- 과거 인터넷 익스플로러 시절 어떤 키가 눌렸는지 확인하기 위해 event.keyCode가 많이 사용됐다.
- 현재는 브라우저 표준이 아닌 코드로 권장하지 않는다.

### 2. event.key 사용하기
- keyCode 대신 key를 사용하면 된다.
- shift 키는 event.shiftKey로 처리 가능하다.
- key property를 지원하지 않는 브라우저도 있어서 예외처리 해주면 좋다.

### 3. 'Shift + Enter = new line / Enter = 전송' 구현하기
```JavaScript
function submitTextarea(event) {
    const key = event.key || event.keyCode;

    if ((key === 'Enter' && !event.shiftKey) || (key === 13 && key !== 16)) {
        event.preventDefault(); // 엔터 키의 기본 역할인 new line 추가를 막는다.

        alert('전송');
    }
}

const $textarea = document.getElementById('my-textarea');
$textarea.addEventListener('keydown', submitTextarea);
// 키가 입력되는 시점에 동작을 막아야하기 떄문에 keyup이 아니라 keydown에 이벤트 줘야한다. 
``` 