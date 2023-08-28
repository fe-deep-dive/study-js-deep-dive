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
1번은 함수 선언문으로, 2번은 함수 표현식으로 선언됨.
1번은 함수 선언문을 자바스크립트 엔진이 해석하면서 동일한 이름의 식별자를 생성하고 할당.
2번은 함수 표현식을 통해 함수 객체가 add라는 변수 식별자에 할당됨.
-->

<br />


> 3. 다음 코드의 실행 결과를 설명하고, 왜 그렇게 동작하는지 설명해라.

```jsx
// 함수 참조
console.dir(add); console.dir(sub); 

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

> 4. 다음 코드의 실행결과를 설명하고, 왜 그렇게 동작하는지 설명해라.

```jsx
function foo() {
  var x = 1;
  var x = 2;
  console.log(x); 
}
```

```jsx
function bar() {
  let x = 1;
  let x = 2;
}
bar();
```


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
