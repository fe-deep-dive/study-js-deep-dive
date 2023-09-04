## 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드로 선언한 변수의 문제점

**변수 중복 선언 허용**
<br>
```javascript
var x = 1;
var y = 1;

var x = 100;  // 중복 선언 허용
var y;        // 초기화문이 없으면 무시됨(에러 발생X)

console.log(x);  // 100
console.log(y);  // 1
```

**함수 레벨 스코프**
<br>
var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
  
if문과 for문에서 var 키워드로 선언한 변수는 전역 변수이다.

```javascript
var i = 1;

if (true) {
  var i = 10;  // 의도치 않게 i 변수의 값이 변경되었다.
}

console.log(i);  // 10

for (var i = 0; i < 5; i++) {
  console.log(i);  // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i);  // 5
```

함수 레벨 스코프는 전역 변수를 남발하여 의도치 않게 전역 변수가 중복 선언될 가능성을 높인다.

**변수 호이스팅**
<br>
var 키워드로 변수를 선언하면 변수 호이스팅에 의해 스코프의 선두로 끌어 올려진 것처럼 동작한다.

변수 선언문 이전에 변수를 참조하는 것은 에러를 발생시키지는 않지만 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

### 15.2 let 키워드
var 키워드의 단점을 보완하기 위해 ES6에서는 let과 const를 도입했다.

**변수 중복 선언 금지**
<br>
let 키워드로 변수를 중복 선언하면 문법 에러<sup>SyntaxError</sup>가 발생한다.
```javascript
let bar = 123;
let bar = 456;  // SyntaxError: Identifier 'bar' has already been declared
```

**블록 레벨 스코프**
<br>
let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프<sup>block-level scope</sup>를 따른다.
```javascript
let foo = 1;
{
  let foo = 2;
  let bar = 3;
}

console.log(foo);  // 1
console.log(bar);  // ReferenceError: bar is not defined
```

**변수 호이스팅**
<br>
let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
  
let 키워드로 선언한 변수를 선언문 이전에 참조하면 참조 에러(ReferenceError)가 발생한다.

```javascript
console.log(foo);  // ReferenceError: foo is not defined
let foo;
```
<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/b130ec28-6667-4bb0-9e73-737b74961086"></p>

var 키워드로 선언한 변수는 런타임 이전에 암묵적으로 "선언 단계"와 "초기화 단계"가 한 번에 진행되어 undefined로 변수를 초기화한다.
  
let 키워드로 선언한 변수는 **"선언 단계"와 "초기화 단계"가 분리되어 진행**된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.
  
스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**<sup>Temporal Dead Zone: TDZ</sup>라고 부른다.

```javascript
let foo = 1;

{
  // 호이스팅이 발생하지 않는다면 전역 변수 foo값을 출력하겠지만
  // let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생한다.
  console.log(foo);  // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2;
}
```

**전역 객체와 let**
<br>
var 키워드로 선언한 변수와 객체는 전역 객체 window의 프로퍼티가 되지만 let 키워드로 선언한 변수는 전역 객체 window의 프로퍼티가 아니다.

```javascript
// 전역 변수
var x = 1;
// 암묵적 전역(선언하지 않은 변수에 값을 할당)
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x);  // 1
console.log(x);         // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y)   // 2
console.log(y);         // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo);// ƒ foo() {}
console.log(foo);       // ƒ foo() {}

let x = 1;

// let 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x);  // undefined
console.log(x);         // 1
```

### 15.3 const 키워드

**선언과 초기화**
<br>
const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
```javascript
const foo = 1;
const bar;  // SyntaxError: Missing initializer in const declaration
```

const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 **블록 레벨 스코프**를 가지며, **변수 호이스팅이 발생하지 않는 것처럼 동작**한다.

**재할당 금지**
<br>
const 키워드로 선언한 변수는 재할당이 금지된다. (재할당시 TypeError가 발생한다.)

**상수**
<br>
const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 이러한 특징을 이용해 const 키워드를 상수(재할당이 금지된 변수)를 표현하는 데 사용하기도 한다.  
  
상수는 **상태 유지와 가독성, 유지보수의 편의**를 위해 적극적으로 사용해야 한다.  

```javascript
// 세율을 의미하는 변수 0.1은 쉽게 바뀌지 않는 값이며, 프로그램 전체에서 고정된 값을 사용해야 한다.
// 상수는 대문자와 스네이크 케이스로 명확하게 표기한다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice);
```


**const 키워드와 객체**
<br>
const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.  
  
const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않는다.

### 15.4 var vs. let vs. const

- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(상수) 원시 값과 객체에는 const 키워드를 사용한다.

**변수를 선언할 때는 일단 const 키워드를 사용하자.** 반드시 재할당이 필요하다면 그때 const 키워드를 let 키워드로 변경해도 늦지 않다.
