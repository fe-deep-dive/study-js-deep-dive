## 31장 RegExp
---

정규 표현식<sup>regular expression</sup>은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 [형식 언어](https://ko.wikipedia.org/wiki/%ED%98%95%EC%8B%9D_%EC%96%B8%EC%96%B4)(formal language)다.

정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다.

```jsx
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 확인한다.
regExp.test(tel);  // false
```

### 31.2 정규 표현식의 생성

정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다. <br>일반적인 방법은 **정규 표현식 리터럴**을 사용하는 것이다.

<p align="center"><img width="30%" src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/31d72ea2-030c-4f5c-99cb-cfa17a253512"></p>

정규 표현식 리터럴은 패턴과 플래그로 구성된다.

[정규 표현식 리터럴]
```jsx
const  target = 'Break time. Kami make that time.';

// 패턴: time
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /time/i;

regexp.test(target);  // true
```

[RegExp 생성자 함수]
```jsx
const  target = 'Break time. Kami make that time.';

const regexp = new RegExp(/time/i);  // ES6
// const regexp = new RegExp(/time/, 'i');
// const regexp = new RegExp('time', 'i');

regexp.test(target);  // true
```

RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.<br>패턴이 변할 가능성이 있는 정규 표현식의 경우 생성자 함수를 사용한다.

```jsx
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Break time. Kami make that time.', 'time');  // 3
count('Break time. Kami make that time.', 'kamisama');  // 0
```

### 31.3 RegExp 메서드

**31.3.1 RegExp.prototype.exec**

exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다. 매칭 결과가 없는 경우 null을 반환한다.

```jsx
const target = 'Break time. Kami make that time.';
const regExp = /time/;

regExp.exec(target); // ['time', index: 5, input: 'Break time. Kami make that time.', groups: undefined]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의해야 한다.

**31.3.2 RegExp.prototype.test**

test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Break time. Kami make that time.';
const regExp1 = /time/;
const regExp2 = /kamisama/;

regExp1.test(target); // true
regExp2.test(target); // false
```

**31.3.3 String.prototype.match**

String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Break time. Kami make that time.';
const regExp1 = /time/;
const regExp2 = /time/g;

target.match(regExp1); // ['time', index: 5, input: 'Break time. Kami make that time.', groups: undefined]
target.match(regExp2); // ['time', 'time']
```

exec 메서드와 다르게 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

### 31.4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.<br>총 6개의 플래그 중 중요한 3개의 플래그를 살펴보자.

|플래그|의미|설명|
|---|---|---|
|i|Ignore case|대소문자를 구별하지 않고 패턴을 검색한다.|
|g|Global|대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.|
|m|Multi line|문자열의 행이 바뀌더라도 패턴 검색을 계속한다.|

플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.<br>플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색하고, 첫 번째 매칭한 대상만 검색한다.

```jsx
const target = 'Break time. Kami make that time.';

// 대소문자 구별O 한 번만 검색
target.match(/time/);

// 대소문자 구별X 한 번만 검색
target.match(/time/i);

// 대소문자 구별O 전역 검색
target.match(/time/g);

// 대소문자 구별X 전역 검색
target.match(/time/ig);
```

### 31.5 패턴

패턴은 `/`로 열고 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색된다.

패턴은 특별한 의미를 가지는 **[메타문자](http://www.ktword.co.kr/abbr_view.php?m_temp1=5851)<sup>meta character</sup> 또는 기호**로 표현할 수 있다.

어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 <strong>'정규 표현식과 매치<sup>match</sup>한다'</strong>고 표현한다.

**31.5.1 문자열 검색**

정규 표현식의 패턴에 문자 또는 문자열을 지정하고 앞서 살펴본 RegExp 메서드를 사용하여 검색 대상 문자열과 정규 표현식의 매칭 결과를 구하면 검색이 수행된다.

플래그를 생략한 정규 표현식과 검색 대상 문자열의 매칭 결과를 구하면 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환한다.

**31.5.2 임의의 문자열 검색**

`.`은 임의의 문자 한개를 의미한다.

```jsx
const target = 'Break time. Kami make that time.';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // ['Bre', 'ak ', 'tim', 'e. ', 'Kam', 'i m', 'ake', ' th', 'at ', 'tim']
```

**31.5.3 반복 검색**

`{m,n}`은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마 뒤에 공백이 있으면 정상 작동하지 않으므로 주의해야 한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

`{n}`은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, `{n}`은 `{n,n}`과 같다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // ['AA', 'AA']
```

`{n,}`은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // 'AA', 'AAA']
```

`+`은 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, `+`는 `{1,}`과 같다.
```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 한번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

`?`는 앞선 패턴이 최대 한 번(0번포함) 이상 반복되는 문자열을 의미한다. 즉, `?`는 `{0,1}`과 같다.
```jsx
const target = 'color colour';

// 'colo' 다음 'u'가 한 번 이상 반복되고 'r'이 이어지는 문자열을 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // ['color', 'colour']
```

**31.5.4 OR 검색**

`|`은 or의 의미를 갖는다.

```jsx
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

```jsx
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A+|B+/g;

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

범위를 지정하려면 `[]` 내에 `-`를 사용한다.

```jsx
const target = 'A AA BB ZZ Aa Bb';

// 'A'~'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Z]+/g;

target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']
```

대소문자를 구별하지 않고 알파벳을 구별하는 방법은 다음과 같다.

```jsx
const target = 'AA BB Aa Bb vV 12';

// 'A'~'Z' 또는 'a'~'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;
// const regExp = /[A-Z]+/ig;

target.match(regExp); // ['AA', 'BB', 'Aa', 'Bb', 'vV']
```

숫자를 검색하는 방법은 다음과 같다.

```jsx
const target = 'AA BB 12,345';

// '0'~'9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9]+/g;

target.match(regExp); // ['12', '345']
```

쉼표로 매칭 결과를 분리하길 원하지 않는다면 쉼표를 패턴에 포함시킨다.

```jsx
const target = 'AA BB 12,345';

// '0'~'9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9,]+/g;

target.match(regExp); // ['12,345']
```

위 예제를 간단히 표현하면 다음과 같다. `\d`는 숫자를 의미한다. `\D`는 `\d`와 반대로 숫자가 아닌 문자를 의미한다.

```jsx
const target = 'AA BB 12,345';

// '0'~'9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // ['12,345']

// '0'~'9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // ['AA BB ', ',']
```

`\w`는 알파벳, 숫자, 언더스코어를 의미한다. 즉, `\w`는 `[A-Za-z0-9_]와 같다. `\W`는 `\w`와 반대로 동작한다. 즉, `\W`는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```jsx
const target = 'Aa Bb 12,345 _$%&';

let regExp = /[\w,]+/g;

target.match(regExp); // ['Aa', 'Bb', '12,345', '_']

regExp = /[\W,]+/g;

target.match(regExp); // [' ', ' ', ',', ' ', '$%&']
```

**31.5.5 NOT 검색**

[...] 내의 `^`은 not의 의미를 갖는다. 예를 들어, `[^0-9]`는 숫자를 제외한 문자를 의미한다.<br>따라서 `[0-9]`와 같은 의미인 `\d`와 반대로 동작하는 `\D`는 `[^0-9]`와 같고, `[A-Za-z0-9_]`와 같은 의미인 `\w`와 반대로 동작하는 `\W`는 `[^A-Za-z0-9_]`와 같다.

```jsx
const target = 'AA BB 12 Aa Bb';

const regExp = /[^0-9]+/g;

target.match(regExp); // ['AA BB ', ' Aa Bb']
```

**31.5.6 시작 위치로 검색**

[...] 밖의 `^`는 문자열의 시작을 의미한다. 단, [...] 내의 `^`은 not의 의미를 가지므로 주의해야 한다.

```jsx
const target = 'https://edussafy.com';

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // true
```

**31.5.7 마지막 위치로 검색**

`$`는 문자열의 마지막을 의미한다.

```jsx
const target = 'https://edussafy.com';

// 'com'으로 끝나는지 검색한다.
const regExp = /com$/;

regExp.test(target); // true
```

<br>

|패턴|의미||
|---|---|---|
|[]|괄호 안의 문자들 중 하나||
|()|그룹화||
|.|임의의 문자 한 개||
|[] 내의 ^|not의 의미||
|[] 밖의 ^|문자열의 시작||
|$|문자열의 마지막||
|{m,n}|최소 m번, 최대 n번 반복||
|{n}|n번 반복|{n,n}|
|{n,}|최소 n번 이상 반복||
|+|최소 한 번 이상 반복|{1,}|
|?|0번 혹은 한 번 반복|{0,1}|
|*|0번 혹은 여러번 반복||
|\||or||
|[] 내의 -|범위 지정||
|\d|숫자||
|\D|숫자가 아닌 문자||
|\w|알파벳, 숫자, 언더스코어|[A-Za-z0-9_]|
|\W|\w의 반대||
|\s|공백 문자(스페이스, 탭 등)|[\t\r\n\v\f]|
|\S|\s가 아닌 문자||


### 31.6 자주 사용하는 정규표현식

**31.6.1 특정 단어로 시작하는지 검사**

```jsx
const url = 'https://jsisweird.com';

//'http://' 또는 'https://'로 시작하는지 검사
/^https?:\/\//.test(url); // true
/^(http|https):\/\//.test(url); // true
```

**31.6.2 특정 단어로 끝나는지 검사**

```jsx
const fileName = 'index.html';

// 'html'로 끝나는지 검사
/html$/.test(fileName); // true
```

**31.6.3 숫자로만 이루어진 문자열인지 검사**

```jsx
const target = '12345';

/^\d+$/.test(target); // true
```

**31.6.4 하나 이상의 공백으로 시작하는지 검사**

`\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, `\s`는 `[\t\r\n\v\f]`와 같은 의미다.

```jsx
const target = ' Hi!';

/^[\s]+/.test(target); // true
```

**31.6.5 아이디로 사용 가능한지 검사**

```jsx
const id = 'abc123';

// 4~10자리의 알파벳 대소문자 또는 숫자로 이루어져 있는지 검사 
/^[A-Za-z0-9]{4,10}$/.test(id);
```

**31.6.6 이메일 주소 형식에 맞는지 검사**

```jsx
const email = 'whattheham@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // true
```

<details>
  <summary>RFC5322 Official Standard</summary>
  <code>(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])</code>
</details>

**31.6.7 핸드폰 번호 형식에 맞는지 검사**

```jsx
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

**31.6.8 특수 문자 포함 여부 검사**


```jsx
const target = 'abc#123';

// 특수 문자는 `A-Za-z0-9` 이외의 문자이다.
(/[^A-Za-z0-9]/gi).test(target); // true

// 특수 문자를 선택적으로 검사할 수 있는 방식
(/[\
[\[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // true
```

특수 문자를 제거할 때에는 String.prototype.replace 메서드를 사용한다.

```jsx
target.replace(/[^A-Za-z0-9]/gi, ''); // abc123
```
