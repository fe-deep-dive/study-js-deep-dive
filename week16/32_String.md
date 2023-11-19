## 32장 String
---

### 32.1 String 생성자 함수

표준 빌트인 객체인 String 객체는 **생성자 함수** 객체다. 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String [래퍼 객체](https://github.com/parkyolo/study-js-deep-dive/blob/study-31/week08/21_%EB%B9%8C%ED%8A%B8%EC%9D%B8%20%EA%B0%9D%EC%B2%B4.md#213-%EC%9B%90%EC%8B%9C%EA%B0%92%EA%B3%BC-%EB%9E%98%ED%8D%BC-%EA%B0%9D%EC%B2%B4)를 생성한다.<br>
인수로 문자열을 전달하면서 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```jsx
let strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}

strObj = new String('Lee');
console.log(strObj); // String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
```

크롬 브라우저의 개발자 도구에서 실행했을 때 보이는 [[PrimitiveValue]] 프로퍼티는 [[StringData]] 내부 슬롯을 가리킨다. ES5에서는 [[StringData]]를 [[PrimitiveValue]]라 불렀다.

String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 [유사 배열 객체](https://github.com/parkyolo/study-js-deep-dive/blob/study-31/week03/11_%EC%9B%90%EC%8B%9C%20%EA%B0%92%EA%B3%BC%20%EA%B0%9D%EC%B2%B4%EC%9D%98%20%EB%B9%84%EA%B5%90.md#111-%EC%9B%90%EC%8B%9C-%EA%B0%92)이면서 이터러블이다. 따라서 **인덱스를 사용하여 각 문자에 접근할 수 있다.**

```jsx
const strObj = new String('Kamisama');
console.log(strObj[0]); // K
```

단, 문자열은 원시 값이므로 **변경할 수 없다.** 이때 에러가 발생하지 않는다.

```jsx
strObj[0] = 'B';
console.log(strObj); // String {'Kamisama'}
```

String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 **문자열로 강제 변환** 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```jsx
let strObj = new String(123);
console.log(strObj); // String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123"}

strObj = new String(null);
console.log(strObj); // String {0: "n", 1: "u", 2: "l", 3: "l", length: 4, [[PrimitiveValue]]: "null"}
```

new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다. 이를 이용하여 [명시적 타입 변환](https://github.com/parkyolo/study-js-deep-dive/blob/study-31/week02/09-%ED%83%80%EC%9E%85%EB%B3%80%ED%99%98.md#93-%EB%AA%85%EC%8B%9C%EC%A0%81-%ED%83%80%EC%9E%85-%EB%B3%80%ED%99%98)을 하기도 한다.

```jsx
String(1);        // "1"
String(NaN);      // "NaN"
String(Infinity); // "Infinity"

String(true);     // "true"
String(false);    // "false"
```

### 32.2 length 프로퍼티

length 프로퍼티는 문자열의 문자 개수를 반환한다.

```jsx
'Break Time'.length; // 10
'쉬는 시간'.length;   // 5
```

### 32.3 String 메서드

배열에는 원본 배열을 직접 변경하는 메서드<sup>mutator method</sup>와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드<sup>accessor method</sup>가 있다.

하지만 String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다. 언제나 새로운 문자열을 반환한다.

문자열을 변경 불가능한<sup>immutable</sup>한 원시 값이기 때문에 **String 래퍼 객체도 읽기 전용<sup>read only</sup> 객체로 제공된다.**

```jsx
const strObj = new String('Fight');

console.log(Object.getOwnPropertyDescriptors(strObj));
/* String 래퍼 객체는 읽기 전용 객체이므로 writable 프로퍼티 어트리뷰트 값이 false다.
{
  0: {value: 'F', writable: false, enumerable: true, configurable: false},
  1: {value: 'i', writable: false, enumerable: true, configurable: false},
  2: {value: 'g', writable: false, enumerable: true, configurable: false},
  3: {value: 'h', writable: false, enumerable: true, configurable: false},
  4: {value: 't', writable: false, enumerable: true, configurable: false},
  length: {value: 5, writable: false, enumerable: false, configurable: false},
}
*/
```

사용 빈도가 높은 String 메서드에 대해 살펴보도록 하자.

**32.3.1 String.prototype.indexOf**

indexOf 메서드는 대상 문자열(메서드를 호출한 문자열)에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

```jsx
const str = 'Vuetiful';

// 'u'를 검색하여 첫 번째 인덱스 반환
str.indexOf('u');  // 1

// 'et'를 검색하여 첫 번째 인덱스 반환
str.indexOf('et'); // 2

// 검색에 실패하면 -1을 반환
str.indexOf('b');  // -1
```

indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```jsx
// str의 인덱스 3부터 'u'를 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('u', 3);  // 6
```

indexOf 메서드는 대상 문자열에 **특정 문자열이 존재하는지 확인**할 때 유용하다.

ES6에서 도입된 String.prototype.includes 메서드를 사용하면 더 가독성이 좋다.

```jsx
if (str.includes('Vue')) {
  // 문자열 str에 'Vue'가 포함되어 있는 경우 처리할 내용
}
```

**32.3.2 String.prototype.search**

search 메서드는 대상 문자열에서 인수로 전달받은 **정규 표현식과 매치하는 문자열**을 검색하여 **일치하는 문자열의 인덱스를 반환**한다. 검색에 실패하면 -1을 반환한다.

```jsx
const str = 'Vuetiful';

str.search(/e/); // 2
str.search(/b/); // -1
```

**32.3.3 String.prototype.includes**

ES6에서 도입된 includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 **포함되어 있는지 확인**하여 그 결과를 **true 또는 false로 반환**한다.

```jsx
const str = 'Vuetiful';

str.includes('Vue'); // true
str.includes('');    // true
str.includes('ham'); // false
str.includes();      // false
```

includes의 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```jsx
str.includes('f', 3); // true
str.includes('e', 3); // false
```

**32.3.4 String.prototype.startsWith**

ES6에서 도입된 startsWith 메서드는 대상 문자열이 **인수로 전달받은 문자열로 시작하는지 확인**하여 그 결과를 **true 또는 false**로 반환한다.

```jsx
const str = 'Vuetiful';

str.startsWith('Vue');  // true
str.startsWith('React');// false
```

startsWith 메서드의 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```jsx
str.startsWith('f', 5); // true
```

**32.3.5 String.prototype.endsWith**

ES6에서 도입된 endsWith 메서드는 대상 문자열이 **인수로 전달받은 문자열로 끝나는지 확인**하여 그 결과를 **true 또는 false**로 반환한다.

```jsx
const str = 'Vuetiful';

str.endsWith('ul'); // true
str.endsWith('li'); // false
```

endsWith 메서드의 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```jsx
str.endsWith('if', 6); // true
```

**32.3.6 String.prototype.charAt**

charAt 메서드는 대상 문자열에서 인수로 전달받은 **인덱스에 위치한 문자를 검색하여 반환**한다.

```jsx
const str = 'Drama';

for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // D r a m a
}
```

인덱스는 문자열의 범위, 즉 0~(문자열 길이-1) 사이의 정수이어야 한다. 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

```jsx
str.charAt(5); // ''
```

charAt 메서드와 유사한 문자열 메서드는 [String.prototype.charCodeAt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)과 [String.prototype.codePointAt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt)이 있다.

<details>
  <summary>charCodeAt vs codePointAt</summary>
  
  - charCodeAt
    
    인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환한다. 범위 밖으로 넘어갔을 경우 NaN을 반환한다.

    ```
    'ABC'.charCodeAt(0); // 65
    'ABC'.charCodeAt(4); // NaN
    ```
    
  - codePointAt
    
    유니코드 코드 포인트 값인 음이 아닌 정수를 반환한다. 범위 밖으로 넘어갔을 경우 undefind를 반환한다.
    
    ```
    '★'.codePointAt(0); // 9733
    '★'.codePointAt(1); // undefind
    ```
</details>

**32.3.7 String.prototype.substring**

substring 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 **부분 문자열을 반환**한다.

```jsx
const str = 'Captin Americano';

str.substring(3, 6); // 'tin'

// 인덱스 7부터 마지막 문자까지 부분 문자열 반환
str.substring(7);    // 'Americano'

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // 'apt'

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2);   // 'Captin Americano'

// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급된다.
str.substring(7, 100);// 'Americano'
str.substring(20);    // ''
```

String.prototype.indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

```jsx
const str = 'Captin Americano';

// 스페이스를 기준으로 앞에 있는 부분 문자열 취득
str.substring(0, str.indexOf(' ')); // 'Captin'

// 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
str.substring(str.indexOf(' ') + 1, str.length); // 'Americano'
```

**32.3.8 String.prototype.slice**

slice 메서드는 **substring 메서드와 동일하게 동작**한다. 단, slice 메서드는 **음수인 인수를 전달할 수 있다**. 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```jsx
cons str = 'Captin Americano';

str.substring(0, 6);  // 'Captin'
str.slice(0, 6);      // 'Captin'

str.substring(3);     // 'tin Americano'
str.slice(3);         // 'tin Americano'

// 인수 < 0 또는 NaN인 경우 0으로 취급
str.substring(-5);    // 'Captin Americano'
// 음수인 인수를 전달할 수 있음. 뒤에서 5자리를 잘라내어 반환
str.slice(-5);        // 'icano'
```

**32.3.9 String.prototype.toUpperCase**

toUpperCase 메서드는 대상 문자열을 **모두 대문자로 변경**한 문자열을 반환한다.

```jsx
const str = 'baNaNa'

str.toUpperCase(); // 'BANANA'
```

**32.3.10 String.prototype.toLowerCase**

toLowerCase 메서드는 대상 문자열을 **모두 소문자로 변경**한 문자열을 반환한다.

```jsx
const str = 'baNaNa'

str.toLowerCase(); // 'banana'
```

**32.3.11 String.prototype.trim**

trim 메서드는 대상 문자열 앞뒤에 **공백 문자가 있을 경우 이를 제거**한 문자열을 반환한다.

```jsx
const str = '  potato  ';

str.trim(); // 'potato'

str.trimStart(); // 'potato  '
str.trimEnd();   // '  potato'
```

String.prototype.replace 메서드에 정규 표현식을 인수로 전달하여 공백 문자를 제거할 수도 있다.

```jsx
const str = '  potato  ';

// 첫 번째 인수로 전달한 정규 표현식에 매치하는 문자열을 두 번째 인수로 전달한 문자열로 치환한다.
str.replace(/\s/g, '');   // 'potato'
str.replace(/^\s+/g, ''); // 'potato  '
str.replace(/\s+$/g, ''); // '  potato'
```

**32.3.12 String.prototype.repeat**

ES6에서 도입된 repeat 메서드는 대상 문자열을 **인수로 전달받은 정수만큼 반복**해 연결한 새로운 문자열을 반환한다.

인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생시킨다. 인수를 생략하면 기본값 0이 설정된다.

```jsx
const str = 'drama';

str.repeat();    // ''
str.repeat(0);   // ''
str.repeat(1);   // 'drama'
// 2.5를 2로 변환
str.repeat(2.5); // 'dramadrama'
str.repeat(-1);  // RangeError: Invalid count value
```

**32.3.13 String.prototype.replace**

replace 메서드는 대상 문자열에서 **첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색**하여 **두 번째 인수로 전달한 문자열로 치환**한 문자열을 반환한다.

```jsx
let str = 'real potato';

str.replace('real', 'fake');  // 'fake potato'

str = 'real real potato';

// 검색된 문자열이 여러 개일 경우 첫 번째로 검색된 문자열만 치환
str.replace('real', 'fake');  // 'fake real potato'

// 특수한 교체 패턴: $&는 검색된 문자열을 의미한다.
str.replace('real', '<strong>$&</strong>'); // '<strong>real</strong> real potato'

// 'real'을 대소문자를 구별하지 않고 전역 검색한다.
str.replace(/real/gi, 'fake'); // 'fake fake potato'
```

<details>
  <summary>교체 패턴</summary>

  |패턴|의미|
  |---|---|
  |$$|"$"를 삽입한다.|
  |$&|검색된 문자열을 삽입한다.|
  |$`|검색된 문자열 이전까지의 문자열을 삽입한다.|
  |$'|검색된 문자열 이후의 문자열을 삽입한다.|
  |$n|n이 1이상 99이하의 정수라면, 첫번째 매개변수로 넘겨진 RegExp객체에서 소괄호로 묶인 n번째의 부분 표현식으로 매치된 문자열을 삽입한다.|

  ```jsx
const str = 'I like apple banana grape.';
console.log(str.replace(/(\w+) banana (\w+)/, '$2 peach $1')); // "I like grape peach apple."
  ```
</details>

replace 메서드의 두 번째 인수로 치환 함수를 전달할 수 있다.

```jsx
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {
  // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
  return camelCase.replace(/.[A-Z]/g, match => {
    console.log(match);  // 'lP'
    return match[0] + '_' + match[1].toLowerCase();
  });
}

const camelCase = 'realPotato';
camelToSnake(camelCase);  // 'real_potato'
```

**32.3.14 String.prototype.split**

split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.

인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```jsx
const str = 'when is the kamisama time?';

str.split(' ');  // ['when', 'is', 'the', 'kamisama', 'time?']
str.split(/\s/); // ['when', 'is', 'the', 'kamisama', 'time?']
str.split('');   // ['w', 'h', 'e', 'n', ' ', 'i', 's', ' ', 't', 'h', 'e', ' ', 'k', 'a', 'm', 'i', 's', 'a', 'm', 'a', ' ', 't', 'i', 'm', 'e', '?']
str.split();     // ['when is the kamisama time?']
```

두 번째 인수로 배열의 길이를 지정할 수 있다.

```jsx
str.split(' ', 3); // ['when', 'is', 'the']
```

split 메서드는 배열을 반환한다. 따라서 Array.prototype.reverse, Array.prototype.join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```jsx
function reverseString(str) {
  return str.split('').reverse().join('');
}

reverseString('소주만병만주소');  // '소주만병만주소'
reverseString('순두부찌개');      // '개찌부두순'
```
