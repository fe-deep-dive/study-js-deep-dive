## 28장: Number

---
Number
- 표준 빌트인 객체
- 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공

### 28.1 Number 생성자 함수

객체 생성자 함수인 Number는 new 연산자와 함께 호출해 인스턴스를 생성할 수 있음

```jsx
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

**[[PrimitiveValue]]는 뭘까?**

[[NumberData]]의 내부 슬롯을 가리킴

> ES5에서는 [[NumberData]]를 [[PrimitiveValue]] 라 불렀음
> 

**[[NumberData]]는 뭘까?**

[[NumberData]] 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성함

- 래퍼객체
    
    원시 타입의 값을 감싸는 형태의 객체
    

```jsx
const numObj = new Number(2024);
console.log(numObj); // Number {[[PrimitiveValue]]: 2024}
```

이때, 인수를 숫자로 변환할 수 없으면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성함 

```jsx
const numObj = new Number('sleeping..zZZ...');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

### new 없이 생성자 함수 호출하기

명시적 타입 변환이 발생해서 Number 인스턴스가 아닌 **숫자**를 반환함. 이를 이용해 명시적으로 타입을 변환할 수 있음.

```jsx
Number('0'); // 0
Number('-1'); // -1
Number('10.52'); // 10.52

Number(true); // 1
Number(false); // 0
```

## 28.2 Number 프로퍼티

**Number.EPSILON**

1과 1보다 큰 숫자 중에서 가장 작은 숫자와 차이가 같음

**활용:** 부동소수점의 산술 연산의 오차 보정

부동소수점을 표현하기 위해 널리 쓰이는 IEEE 754는 2진법으로 변환했을 때 무한 소수가 되어 미세한 오차가 발생함	

```jsx
0.1 + 0.2; // 0.30000000000000004
0.1 + 0.2 === 0.3; // false
```

```jsx
// a와 b를 뺀 절대값이 Number.EPSLION 보다 작으면 같은 수로 인정
function isEqual(a, b){
	return Math.abs(a-b) < Number.EPSLION;
}

isEqual(0.1+0.2, 0.3); // true
```

**Number.MAX_VALUE**

자바스크립트에서 표현할 수 있는 가장 큰 양수 값.

Number.MAX_VALUE보다 큰 숫자는 INFINITY임.

```jsx
Number.MAX_VALUE; // 3157e+308
INFINITY > Number.MAX_VALUE; // true
```

**Number.MIN_VALUE**

자바스크립트에서 표현할 수 있는 가장 작은 양수 값.

Number.MIN_VALUE보다 작은 숫자는 0임

```jsx
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0; // true
```

**Number.MAX_SAFE_INTEGER**

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값 .

- 안전하다?
    
    > IEEE 754를 사용하기 때문에 `-(2^53 - 1)`과 `2^53 - 1` 사이의 수만 안전하게 표현할 수있음.
    https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER
    > 

```jsx
Number.MAX_SAFE_INTEGER; // 900719925474099
```

**Number.MIN_SAFE_INTEGER**

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값.

```jsx
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

**Number.POSITIVE_INFINITY**

양의 무한대를 나타내는 숫자값 Infinity와 같음.

```jsx
Number.POSITIVE_INFINITY; // Infinity
```

**Number.NEGATIVE_INFINITY**

음의 무한대를 나타내는 숫자값 -Infinity와 같음.

```jsx
Number.NEGATIVE_INFINITY; // -Infinity
```

**Number.NaN**

Not-a-Number인 숫자가 아닌 숫자값. window.NaN과 같음.

```jsx
Number.NaN; // NaN
```

## 28.3 Number 메서드

**Number.isFinite**

ES6에서 도입된 메서드.

인수로 전달된 숫자값이 정상적인 유한수, 즉Infinity 또는 -Infinity가 아닌 값인지 검사하고 boolean을 반환

```jsx
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
```

만약 인수가 NaN이면 언제나 false를 반환함.

```jsx
Number.isFinite(NaN); // false
```

**Number.isFinite vs isFinite**

Number.isFinite: 암묵적 타입 변환 X

isFinite: 암묵적 타입 변환 O

```jsx
Number.isFinite(true); // false
isFinite(true); // true

Number.isFinite(null); // false
isFinite(null); // true
```

**Number.isInteger**

ES6에서 도입된 메서드.

인수로 전달된 숫자값이 정수인지 검사하여 불리언 값을 반환함. 암묵적 타입 변환하지 않음.

```jsx
Number.isInteger(0); // true
Number.isInteger(123); // true
Number.isInteger(-123); // true

Number.isInteger(0.5); // false
Number.isInteger('123'); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
Number.isInteger(-Infinity); // false
```

**Number.isNaN**

ES6에서 도입된 메서드.

인수로 전달된 숫자값이 NaN인지 검사하여 불리언 값을 반환함. 

```jsx
Number.isNaN(NaN); // true
```

**Number.isNaN vs isNaN**

Number.isNaN: 암묵적 타입 변환X

isNaN: 암묵적 타입 변환O

```jsx
Number.isNaN(undefined); // false
isNaN(undefined); // true
```

**Number.isSafeInteger**

ES6에서 도입된 메서드.

인수로 전달된 숫자값이 안전한 **정수**인지 검사해 불리언 결과를 반환함.

안전한 정수값은 -(2^53-1)과 2^53-1 사이의 정수값임.

암묵적 타입 변환 X

```jsx
Number.isSafeInteger(0); // true
Number.isSafeInteger(Math.pow(2,53)-1); // true
Number.isSafeInteger(Math.pow(2,53)); // false

// 정수가 아니다.
Number.isSafeInterger(0.5); // false

// 암묵적 타입 변환하지 않는다.
Number.isSafeInteger('123'); // false
Number.isSafeInteger(false); // false
Number.isSafeInteger(Infinity); // false
```

**Number.prototype.toExponential**

숫자를 지수 표기법으로 변환해 문자여 반환함.

- 지수 표기법이란?
    
    지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 사용함. e앞에 있는 숫자에 10^n승을 곱하는 형식으로 수를 나타냄.
    

```jsx
(100).toExponential(); // '1e+2'
```

인수로 소수점 이하로 표현할 자릿수를 전달할 수 있음.

```jsx
(100.12345).toExponential**(2); // '1.00e+2'**
(100.12345).toExponential**(3); // '1.001e+2'**
(100.12345).toExponential**(4); // '1.0012e+2'**
```

참고로 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생함.

```jsx
100.toExponential(); // SyntaxError: Invalid or unexpected token
```

**숫자 뒤의 .의 의미 해석하는법**

자바스크립트는 숫자 뒤의 .를 부동 소수점 숫자의 소수 구분 기호로 해석한다.

```jsx
// 100은 Numbera 래퍼 객체임
// 소수 구분 기호로 해석함
// 따라서 .을 소수 구분 기호로 해석해 에러가 발생함
100.toExponential(); // SyntaxError: Invalid or unexpected token
```

아래 예제에서 두번째 .은 프로퍼티 접근 연산자로 해석된다.

```jsx
// 첫번째 .은 숫자가 오므로 소수 구분 기호자임
// 두번째 .은 프로퍼티 접근 연산자로 해석함 
100.12345.toExponential(); // '1.0012345e+2'
```

혼란을 방지하기 위해 그룹 연산자를 사용하자.

```jsx
(100).toExponential(); // '1e+2'
(100.12345).toExponential(); // '1.0012345e+2'
```

~~다음 같은 방법도 허용되기는 한다.~~ 

```jsx
100 .toExponential(); // '1e+2'
```

**Number.prototype.toFixed**

숫자를 **반올림**하여 문자열로 반환한다.

반올림하는 **소수점 이하 자릿수**를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.

```jsx
(12345.6789).toFixed(); // '123456'

// 소수점 이하 1자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1); // '12345.7'

// 소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1); // '12345.68'
```

**Number.prototype.toPrecision**

인수로 전달받은 **전체 자릿수**까지 유효하도록 나머지 자릿수를 **반올림**하여 문자열로 반환한다. 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.

전체 자릿수를 나타내는 0~21사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.

```jsx
(12345.6789).toPrecision(); // '12345.6789'

// 전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // '1e+4'

// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // '1.2e+4'

// 전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // '1.23457e+4'
```

**Number.prototype.toString**

숫자열을 **문자열**로 변환해 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 10진법이 지정된다.

```jsx
// 인수를 생략하면 10진수 문자열을 반환
(10).toString(); // '10'

// 2진수 문자열을 반환
(16).toString(2); // '1000'

// 8진수 문자열을 반환
(16).toString(8); // '20'

// 16진수 문자열을 반환
(16).toString(16); // '10'
```
