# RegExp

> 프로젝트를 진행하면서 회원가입과 로그인을 할 때 유효성 검사를 진행해야 했는데 이를 위해 **정규 표현식**을 사용하려고 한다. 그래서 그간 피하기만 했던 정규 표현식을 공부해 보려고 한다.

<br />

## 1. 정규 표현식

- 정규 표현식(regular expression): 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)
- 문자열을 대상으로 **패턴 매칭 기능**을 제공: 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있다.
- 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

### 1.1. 사용 예시

- 텍스트에서 우리가 원하는 특적한 패턴을 찾을 때
  - 전화번호 형태의 패턴
  - 웹사이트 형태의 패턴
- 찾은 패턴을 다른 문자열로도 변환 할 수 있다.
- 이메일이나 패스워드 같은 특정한 패턴에 부합하는지 유효성 검사를 할 때도 사용

## 2. 정규 표현식 생성

- **정규 표현식 리터럴**, RegExp 생성자 함수 사용

```javascript
// 정규 표현식 리터럴
// 패턴: is
// 플래그: i
const regexp = /is/i;

// RegExp 생성자 함수
/**
 * pattern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
*/
new RegExp(pattern[, flags])
```

## 3. RegExp 메서드

### 3.1. RegExp.prototype.exec

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
- 매칭 결과가 없는 경우 null 반환
- 문자열 내의 모든 패턴을 검색하는 `g`플래그를 지정해도 첫 번째 매칭 결과만 반환

```javascript
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

### 3.2. RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환

```javascript
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // -> true
```

### 3.3. String.prototype.match

- 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
- exec 메서드와 다르게 `g`플래그가 지정되면 모든 매칭 결과를 배열로 반환

```javascript
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // -> ["is", "is"]
```

### 3.4. String.prototype.replace

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환
- `$&`는 검색된 문자열을 의미, `$(숫자)`는 검색된 그룹들을 의미한다.

```javascript
const str = "Hello world";

str.replace("world", "<strong>$&</strong>"); // -> Hello <strong>world</strong>
```

```javascript
const str = "coding everything";
const regExp = /(\w+)\s(\w+)/g;

str.replace(regExp, "$2, $1"); // -> everything, coding
```

- 두번째 인자로 치환 함수를 전달할 수도 있다. 치환 함수의 첫번째 이수로는 매치한 결과가 들어오게 된다.

## 4. 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용
- 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능
- 어떠한 플래그도 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색
- 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료

| 플래그 | 의미        | 설명                                                       |
| ------ | ----------- | ---------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색                       |
| g      | Global      | 대산 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속                  |

```javascript
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색
target.match(/is/i);
// -> ["is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색
target.match(/is/gi);
// -> ["Is", "is", "is"]
```

## 5. 패턴

- 문자열의 일정한 규칙을 표현하기 위해 사용
- `/`로 열고 닫으며 따옴표는 생략(따옴표를 사용하면 따옴표도 패턴에 포함됨)
- 메타문자또는 기호로 표현할 수 있다.

### 5.1. 문자열 검색

- 패턴에 문자 또는 문자열을 지정
- 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색

```javascript
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴.
const regExp = /is/;

regExp.test(target);

target.match(regExp);
```

- 특수 문자인 경우 `\`를 앞에 붙여서 이스케이프 문자로 만들어서 패턴에서 사용되는 기호와 구분 한다.

```javascript
const target = ".[]{}()^$|?*+";

const regExp = /\[/;

target.match(regExp); // -> ["["]
```

### 5.2. 임의의 문자열 검색

- `.`은 임의의 문자 한 개를 의미

```javascript
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색
const regExp = /.../g;

target.match(regExp);
// -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 5.3. 반복 검색

- `{m,n}`은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미
- 콤파 뒤에 공백이 있으면 정상 동작하지 않음

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

- `{n}`은 앞선 패턴이 n번 반복되는 문자열을 의미
- `{n}`은 `{n,n}`과 같다.

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 2번 반복되는 문자열을 전역 검색
const regExp = /A{2}/g;

target.match(regExp); // -> ["AA", "AA"]
```

- `{n,}`은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

- `+`는 앞선 패턴이 최소 한번 이상 반복되는 문자열
- `+`는 `{1,}`과 같다.

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /A+/g;

target.match(regExp); // -> ["A", "AA", "A", "AAA"]
```

- `?`는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미
- `?`는 `{0,1}`과 같다.

```javascript
const target = "color colour";

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
// 문자열 'color', 'colour'를 전역 검색
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

- `*`는 앞선 패턴이 없거나 최소 한번 이상 반복되는 문자열

```javascript
const target = "gray gry graay graaay";

const regExp = /gra*y/g;

target.match(regExp); // -> ["gray", "gry", "graay", "graaay"]
```

- `?`가 반복 패턴(수량자) 뒤에 오게 되면 반복 패턴의 범위중 가장 작은 숫자를 의미하게 된다.

```javascript
const target = "gray gry graay graaay";

// '*'의 범위중 가장 작은 숫자는 0이므로 '*?'는 0개가 반복되는 것을 의미한다.
const regExp = /g.*?/g;

target.match(regExp); // -> ["g", "g", "g", "g"]
```

```javascript
const target = "gray gry graay graaay";

// '+'의 범위중 가장 작은 숫자는 1이므로 '+?'는 1개가 반복되는 것을 의미한다.
const regExp = /g.+?/g;

target.match(regExp); // -> ["gr", "gr", "gr", "gr"]
```

- 다음과 같은 상황에서 활용해서 사용된다.

```javascript
const target = "<div>test</div><div>test2</div>";

const regExp = /<div>.+<\/div>/g;

target.match(regExp); // -> ["<div>test</div><div>test2</div>"]
```

```javascript
const target = "<div>test</div><div>test2</div>";

const regExp = /<div>.+?<\/div>/g;

target.match(regExp); // -> ["<div>test</div>", "<div>test2</div>"]
```

### 5.4. OR 검색

- `|`은 or을 의미

```javascript
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색
const regExp = /A|B/g;

target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
```

- 분해되지 않은 단어 레벨로 검색하기 위해서는 `+`와 함께 사용

```javascript
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /A+|B+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

- `[]`내의 문자는 or로 동작

```javascript
const target = "A AA B BB Aa Bb";

// 위의 예제와 똑같이 구현
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

- `[]`내에 `-`를 사용해서 범위를 지정

```javascript
const target = "A AA BB ZZ Aa Bb";

// 'A' ~ 'Z'(대문자 알파벳)가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Z]+/g;

target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]
```

```javascript
const target = "AA BB Aa Bb 12";

// 'A' ~ 'Z' 또는 'a' ~ 'z'(대소문자 구별하지 않은 알파벳)가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
```

```javascript
const target = "AA BB 12,345";

// '0' ~ '9'(숫자)가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[0-9]+/g;

target.match(regExp); // -> ["12", "345"]
```

```javascript
const target = "AA BB 12,345";

// '0' ~ '9'(숫자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[0-9,]+/g;

target.match(regExp); // -> ["12,345"]
```

- `\d`는 숫자, `\D`는 숫자가 아닌 문자를 의미

```javascript
const target = "AA BB 12,345";

// 위의 예제와 똑같이 구현
const regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]
```

- `\w`는 알파벳, 숫자, 언더스코어를 의미 (`\w`는 `[A-Za-z0-9_`와 같다)
- `\W`는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미

```javascript
const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", "$%&"]
```

### 5.5. NOT 검색

- `[]`내의 `^`은 not을 의미한다.
- `\D`는 `[^0-9]`와 같다.
- `\W`는 `[^A-Za-z0-9_]`와 같다.

```javascript
const target = "AA BB 12 Aa Bb";

// 숫자를 제외한 문자열을 전역 검색
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB Aa Bb"]
```

### 5.6. 시작 위치로 검색

- `[]` 밖의 `^`은 문자열의 시작을 의미
- 플래그에서 `m` 멀티 라인을 넣어주면 라인 별로 검색

```javascript
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사
const regExp = /^https/;

regExp.test(target); // -> true
```

- `\A`도 문자열의 시작을 의미하지만 플래그의 `m`을 사용해도 옵션이 적용되지 않는다.

### 5.7. 마지막 위치로 검색

- `$`는 문자열의 마지막을 의미
- 플래그에서 `m` 멀티 라인을 넣어주면 라인 별로 검색

```javascript
const target = "https://poiemaweb.com";

// 'com'으로 끝나는지 검사
const regExp = /com$/;

regExp.test(target); // -> true
```

- `\Z`도 문자열의 마지막을 의미하지만 플래그의 `m`을 사용해도 옵션이 적용되지 않는다.

### 5.8. 단어의 경계

- `\b`는 단어의경계를 의미 (boundary를 의미)

```javascript
const target = "Ya ya YaYaYa Ya";

// 단어 앞에서 사용되는 'Ya'만 검색
const regExp = /\bYa/g;

// 가운데 'YaYaYa' 중 시작하는 'Ya'를 찾는다.
target.match(regExp); // -> ["Ya", "Ya", "Ya"]
```

```javascript
const target = "Ya ya YaYaYa Ya";

// 단어 뒤에서 사용되는 'Ya'만 검색
const regExp = /Ya\b/g;

// 가운데 'YaYaYa' 중 끝나는 'Ya'를 찾는다.
target.match(regExp); // -> ["Ya", "Ya", "Ya"]
```

- `\B`는 `\b`와 반대로 동작

```javascript
const target = "Ya ya YaYaYa Ya";

// 단어 뒤에서 사용되지 않는 'Ya'만 검색
const regExp = /Ya\B/g;

// 가운데 'YaYaYa' 중 시작하는 'Ya'와 중간에 위치한 'Ya'를 찾는다.
target.match(regExp); // -> ["Ya", "Ya"]
```

### 5.9. 그룹

- `()`를 사용하여 그룹을 묶어 줄 수 있다.

```javascript
const target = "Hi there, Nice to meet you. And Hello.";

const regExp = /(Hi|Hello)|(And)/g;

target.match(regExp);
// -> ['Hi', 'And', 'Hello']
// 'Hi'와 'Hello'는 앞그룹, 'And'는 뒷 그룹에 속한다.
```

- `()`안에 `?:`를 사용하면 그룹을 기억하지는 않는다.

```javascript
const target = "grey(gray)";

const regExp = /gr(?:a|e)y/g;

target.match(regExp); // -> ['grey', 'gray']
```

## 6. 전방/후방 탐색

- `?=`(전방 탐색)와 `?<=`(후방 탐색)는 뒤에오는 패턴을 검색하는데는 사용하지만 결과에서는 제외한다.

```javascript
const target = "AAAX---aaax---111";

const regExp = /\w+(?=X)/g;

target.match(regExp); // -> ['AAA']
```

```javascript
const target = "AAAX---aaax---111";

const regExp = /\w+(?=\w)/g;

target.match(regExp); // -> ['AAA', 'aaa', '11']
```

```javascript
const target = "XAAA---xaaa---111";

const regExp = /(?<=X)\w+/g;

target.match(regExp); // -> ['AAA']
```

```javascript
const target = "XAAA---xaaa---111";

const regExp = /(?<=\w)\w+/g;

target.match(regExp); // -> ['AAA', 'aaa', '11']
```

- `=`대신 `!`를 사용해서 부정형으로 사용할 수도 있다.

```javascript
const target = "I paid $30 for 100 apples.";

const regExp = /(?<=\$)\d+/g;

target.match(regExp); // -> ['30']
```

```javascript
const target = "I paid $30 for 100 apples.";

// \b 넣어서 '$30'의 '0'은 검색되지 않게 한다.
const regExp = /\b(?<!\$)\d+/g;

target.match(regExp); // -> ['100']
```

## 7. 자주 사용하는 정규표현식

### 7.1. 숫자로만 이루어진 문자열인지 검사

```javascript
// 처음과 끝 모두 숫자이고 최소 한 번 이상 반복되는 문자열
// === 숫자로만 이루어진 문자열
/^\d+$/.test("12345"); // -> true
```

### 7.2. 하나 이상의 공백으로 시작하는지 검사

- `\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미
- [\t\r\n\v\f]와 같은 의미

```javascript
// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(" Hi!"); // -> true
```

### 7.3. 원하는 데이터 찾아서 가지고 오기

```javascript
const url = "https://www.youtu.be/-ZClicWm0zM";

// 데이터로 가지고 올 필요 없는 그룹은 ?: 를 붙여준다.
const regExp = /(?:https?:\/\/)?(?:www\.)?youtu.be\/([a-zA-Z0-9-]{11})/;

url.match(regExp);
// -> ['https://www.youtu.be/-ZClicWm0zM', '-ZClicWm0zM', index: 0, input: 'https://www.youtu.be/-ZClicWm0zM', groups: undefined]
// 배열을 첫번째 요소로 매칭되는 결과의 전체 문자열이 들어온다.
// 그 다음 부터 매칭된 그룹의 데이터가 들어오게된다.

const result = url.match(regExp);

result[1]; // -> -ZClicWm0zM
```

## 8. 문법 정리

### 8.1. Groups and ranges

| Character | 뜻                                     |
| --------- | -------------------------------------- |
| `\|`      | 또는                                   |
| `()`      | 그룹                                   |
| `[]`      | 문자셋, 괄호안의 어떤 문자든           |
| `[^]`     | 부정 문자셋, 괄호안의 어떤 문가 아닐때 |
| `(?:)`    | 찾지만 기억하지는 않음                 |

### 8.2. Quantifiers

| Character   | 뜻                                  |
| ----------- | ----------------------------------- |
| `?`         | 없거나 있거나 (zero or one)         |
| `*`         | 없거나 있거나 많거나 (zero or more) |
| `+`         | 하나 또는 많이 (one or more)        |
| `{n}`       | n번 반복                            |
| `{min,}`    | 최소                                |
| `{min,max}` | 최소, 그리고 최대                   |

### 8.3. Boundary-type

| Character | 뜻               |
| --------- | ---------------- |
| `\b`      | 단어 경계        |
| `\B`      | 단어 경계가 아님 |
| `^`       | 문장의 시작      |
| `$`       | 문장의 끝        |

### 8.4. Character classes

| Character | 뜻                           |
| --------- | ---------------------------- |
| `\`       | 특수 문자가 아닌 문자        |
| `.`       | 어떤 글자 (줄바꿈 문자 제외) |
| `\d`      | digit 숫자                   |
| `\D`      | digit 숫자 아님              |
| `\w`      | word 문자                    |
| `\W`      | word 문자 아님               |
| `\s`      | space 공백                   |
| `\S`      | space 공백 아님              |

## 9. Reference

- [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567)
- [정규표현식, 더이상 미루지 말자](https://www.youtube.com/watch?v=t3M6toIflyQ) 유튜브 영상
- ['dream-ellie' GitHub의 'regex' repo README](https://github.com/dream-ellie/regex)
- [생활코딩의 '정규표현식 패턴들'](https://opentutorials.org/course/909/5143)
- [전방탐색과 후방탐색](https://junstar92.tistory.com/373)
