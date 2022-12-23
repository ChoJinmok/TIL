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

```javascript
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사
const regExp = /^https/;

regExp.test(target); // -> true
```

### 5.7. 마지막 위치로 검색

- `$`는 문자열의 마지막을 의미

```javascript
const target = "https://poiemaweb.com";

// 'com'으로 끝나는지 검사
const regExp = /com$/;

regExp.test(target); // -> true
```

### 5.8. 그룹

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

## 6. 자주 사용하는 정규표현식

### 6.1. 숫자로만 이루어진 문자열인지 검사

```javascript
// 처음과 끝 모두 숫자이고 최소 한 번 이상 반복되는 문자열
// === 숫자로만 이루어진 문자열
/^\d+$/.test("12345"); // -> true
```

### 6.2. 하나 이상의 공백으로 시작하는지 검사

- `\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미
- [\t\r\n\v\f]와 같은 의미

```javascript
// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(" Hi!"); // -> true
```

## 7. 문법 정리

### 7.1. Groups and ranges

| Character | 뜻                                     |
| --------- | -------------------------------------- |
| `\|`      | 또는                                   |
| `()`      | 그룹                                   |
| `[]`      | 문자셋, 괄호안의 어떤 문자든           |
| `[^]`     | 부정 문자셋, 괄호안의 어떤 문가 아닐때 |
| `(?:)`    | 찾지만 기억하지는 않음                 |

### 7.2. Quantifiers

| Character   | 뜻                                  |
| ----------- | ----------------------------------- |
| `?`         | 없거나 있거나 (zero or one)         |
| `*`         | 없거나 있거나 많거나 (zero or more) |
| `+`         | 하나 또는 많이 (one or more)        |
| `{n}`       | n번 반복                            |
| `{min,}`    | 최소                                |
| `{min,max}` | 최소, 그리고 최대                   |

### 7.3. Boundary-type

| Character | 뜻               |
| --------- | ---------------- |
| `\b`      | 단어 경계        |
| `\B`      | 단어 경계가 아님 |
| `^`       | 문장의 시작      |
| `$`       | 문장의 끝        |

### 7.4. Character classes

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

## 8. Reference

- [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567)
- [정규표현식, 더이상 미루지 말자](https://www.youtube.com/watch?v=t3M6toIflyQ) 유튜브 영상
- ['dream-ellie' GitHub의 'regex' repo README](https://github.com/dream-ellie/regex)
