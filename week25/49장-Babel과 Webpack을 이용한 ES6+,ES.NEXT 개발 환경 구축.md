# 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축
트랜스파일러인 Babel과 모듈 번들러인 Webpack으로 구형 브라우저에서 동작하는 ES5 사양의 코드로 트랜스파일링할 수 있음 

# ES6 이하 지원하기
크롬, 사파리, 파이어폭스, 엣지 같은 에버그린 브라우저의 ES6 지원율은 약 98%로 거의 대부분 지원함

ES6를 지원해야하는 이유
- IE 11의 지원율은 약 11%
- ES6+(매년 새롭게 도입되는 버전)과 제안 단계에 있는 ES.NEXT 지원은 브라우저에 따라 지원율이 제각각

IE를 포함한 구형 브라우저에서 문제 없이 동작하는 개발 환경을 구축해야 함 

# 모듈 로더 지원하기
대부분 프로젝트가 모듈을 사용하므로 모듈 로더가 필요함
ES6 모듈(ESM)은 대부분의 모던 브라우저(Chrome 61, FF 60, SF 10.1, Edge 16이상)에서 사용 가능

모듈 로더를 지원해야하는 이유
- IE를 포함한 구형 브라우저는 ESM 지원 X
- ESM을 사용하더라도 트랜스파일링이나 번들링이 필요함
- ESM이 아직 지원하지 않는 기능(bare import 등)이 있고 점차 해결되고 있으나 이슈가 존재함
  

# Babel

ES6의 화살표 함수와 ES7의 지수 연산자를 사용하고 있음
```jsx
[1, 2, 3].map(n => n ** n);
```

구형 브라우저를 위해 Babel을 사용해 ES5로 변환할 수 있음
```jsx
"use strict"

[1, 2, 3].map(function (n) {
    return Math.pow(n, n);
});
```
Babel을 통해 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 구형 브라우저에 동작하도록 트랜스파일링 할 수 있음

<img width="1076" alt="스크린샷 2024-02-05 오후 9 41 53" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/9945c0ae-84ff-479a-8103-6ba4a84430cc">

### 트랜스파일링

(과정 생략)

```jsx
// src/js/lib.js
export const pi = Math.PI; // ES6 모듈

export function power(x, y) {
  return x ** y; // ES7 지수 연산자
}

// ES6 클래스
export class Foo {
  #private = 10; // stage 3: 클래스 필드 정의 제안

  foo() {
    // stage 4: 객체 Rest/Spread 프로퍼티 제안
    const { a, b, ...x } = { ...{ a: x, b: 2 }, c: 3, d: 4 };
    return { a, b, x };
  }

  bar() {
    return this.#private;
  }
}

// src/js/main.js
import { Foo, pi, power } from './lib';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```

**트랜스파일링 결과**

<img width="1076" alt="스크린샷 2024-02-05 오후 9 41 53" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/d7aa782a-9e6a-45c8-8329-f0585b92b875">

<img width="689" alt="스크린샷 2024-02-05 오후 9 42 31" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/f759ddb4-d2aa-4a14-8657-ce069bacb6e2">


브라우저에서 실행할 경우 CommonJS 방식의 require 함수를 지원하지 않으므로 에러가 발생함

<img width="420" alt="스크린샷 2024-02-05 오후 9 42 52" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/0bc64a0e-6665-4923-9265-1fdf296199cb">


### Webpack
의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스를 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러

장점
- 의존 모듈이 하나의 파일로 번들링 되므로 별도의 모듈 로더가 필요 없음
- 여러 개의 자바스크립트의 파일을 하나로 번들링하므로 script 태그로 여러 개의 자바스크립트 파일을 로드하지 않아도 됨

과정:
Babel을 로드해 트랜스파일링(`babel-loader`) → Webpack이 자바스크립트 파일 번들링

**babel-polyfill**

브라우저가 지원하지 않는 코드가 남아 있을 수 있음 (예: Promise, Object.assign, Array.from 등)

ES5에 대체할 기능이 없어 polyfill 해줘야 함 

**번들링 결과**
<img width="846" alt="스크린샷 2024-02-05 오후 9 51 30" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/d3768046-d90f-49cd-9545-2b35bb2478b5">

<details>
<summary> polyfill이란?</summary>
브라우저 간 호환성을 유지하고 새로운 기능을 구현
브라우저 간의 차이로 인해 발생하는 호환성 문제를 해결하기 위해 사용됨
</details>

