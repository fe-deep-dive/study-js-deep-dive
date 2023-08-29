## Quiz

- 진행자 : 김민정
- 날짜 : Aug 29 2023

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

<br />

> 1. 함수를 정의하는 방법 4가지의 이름을 채워라.

**함수 정의 방식**
| 함수 정의 방식 | 예시 |
| - | - |
| ??? | `fucnction add(x, y) { return x + y; }`|
| ??? | `var add = function(x, y) { return x + y; };`|
| ??? | `var add = new Function('x', 'y', 'return x+y');`|
| ??? | `var add = (x, y) => x + y;` |

<!--
함수 선언문, 함수 표현식, Function 생성자 함수, 화살표 함수
-->

<br />

> 2. 다음 두 개의 함수 정의 방식의 차이점과, 자바스크립트 엔진이 이를 어떻게 설명해라.

```jsx
// 1
function add(x, y) {
  return x + y;
}

// 2
var add = function add(x, y) {
  return x + y;
};
```

<!--
1번은 함수 선언문으로, 2번은 함수 표현식으로 함수를 정의했다.
1번은 함수 선언문으로서 함수 호이스팅이 일어나고, 자바스크립트 엔진이 함수 객체를 생성하며 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 이에 함수 객체를 할당한다.
2번은 함수 표현식으로서 변수 호이스팅이 일어나고, 값이 평가될 때 함수 객체가 생성되어 변수에 할당된다. 함수 표현식을 통해 함수 객체가 add라는 변수 식별자에 할당됨.
-->

<br />


> 3. 다음 코드의 실행 결과를 설명하고, 왜 그렇게 동작하는지 설명해라.

```jsx
// 함수 참조
console.dir(add);
console.dir(sub); 

// 함수 호출
console.log(add(2, 5));
console.log(sub(2, 5));

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

<!--
console.dir 함수는 함수의 파라미터까지 출력하고, console.log는 함수 객체를 출력한다는 차이점이 있다.
다만 node.js 런타임에서는 dir 함수도 log와 같이 출력된다.

함수 선언문에서는 함수 호이스팅이 일어나 console.dir(add) 시 f add(x, y) 가 출력되고 console.log(add(2, 5))시 함수가 실행되어 반환값인 7이 출력된다.
한수 표현식에서는 변수 호이스팅이 일어나 sub 식별자가 undefined로 초기화 되어 console.dir(sub) 시 undefined가 출력된다.
console.log(sub(2, 5)) 에서는 undefined를 함수로서 호출하기 때문에 TypeError가 발생한다.
-->

> 4. 다음 코드의 실행결과를 설명하고, 왜 그렇게 동작하는지 설명해라.

```jsx
function foo() {
  var x = 1;
  var x = 2;
  console.log(x); 
}
foo();
```

```jsx
function bar() {
  let x = 1;
  let x = 2;
  console.log(x); 

}
bar();
```

<!--
foo 함수에서는 foo 함수 스코프의 지역변수인 x가 재선언이 가능한 키워드인 var를 사용했기 때문에 2가 출력된다.
bar 함수에서는 bar 함수 스코프의 지역변수인 x가 재선언이 불가한 키워드인 let을 사용하여 SntaxError: Identifier 'x' has already been declared가 발생한다.
-->

> 5. 다음 두 개의 `foo` 함수는 어떤 차이를 가지는지 설명하고, 실행 결과를 설명해라.

```jsx
function foo() {
  console.log("first");
}

function bar() {
  function foo() {
    console.log("second");
  }
  foo(); 
}

bar();
```

<!--
전역스코프에 foo라는 함수가 존재하고, bar라는 함수가 존재한다.
bar라는 함수 스코프에는 foo라는 지역 함수가 존재하고, bar 내부에서 foo를 참조하게 되면 지역 함수인 foo가 호출되어 second가 출력된다.
-->