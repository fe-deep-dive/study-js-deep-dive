## 20장: strict mode

---

### **20.1 strict mode란?**

먼저 암묵적 전역<sup>implicit global</sup>을 알아보자.

**암묵적 전역<sup>implicit global</sup>**

```jsx
const add = (a, b) => {
  x = 10;
  return a + b;
};

console.log(x); // ?
add(1, 2);
console.log(x); // 10, arrow function에서도 작동
```

변수를 키워드 없이 초기화한 경우 자바스크립트 엔진이 **암묵적으로** 전역 객체로 동적 생성한다.

> 따라서, 그럴리는 없겠지만 `const`, `let` 을 까먹지 말자.

</br>

**strict mode(엄격 모드)**

<img src='../images/strict.png' width=100px>

</br>

키워드를 까먹는 개발자를 위해 es5부터 도입된 모드

자바스크립트 문법을 `엄격`하게 적용하여 명시적인 에러를 발생시킨다.

~~하지만 이 글의 저자는 ESLint를 추천하고 있으며 저도 Lint가 고쳐주고 있습니다.~~

</br>

### **20.2 strict mode의 적용**

~~스포일러: 두 방법 다 쓰지 말라고 한다~~

**전역의 선두**

- 스크립트 전체 적용

```jsx
"use strict";

const add = (a, b) => {
  x = 10;
};
add(1, 2); // ReferenceError: x is not defined
```

**함수 몸체의 선두**

- 함수와 중첩 함수에 적용

```jsx
const add = (a, b) => {
  "use strict";
  x = 10;
};
add(1, 2); // ReferenceError: x is not defined
```

</br>

### **20.3 전역에 strict mode를 적용하는 것은 피하자**

### ?

이상한 예시 하나를 보자.

```jsx
<html>
  <head></head>
  <body>
    <script>'use strict' x = 1; // ReferenceError: x is not defined</script>
    <script>y = 1; // 에러가 발생하지 않는다.</script>
  </body>
</html>
```

strict mode 스크립트와 non-strict mode 스크립트를 같이 쓰면 오류가 있을 수 있다고 한다...

### **20.4 함수 단위로 strict mode를 적용하는 것도 피하자**

### ? ?

또 이상한 예시를 하나 보자.

```jsx
(function () {
  // 'use strict'
  var let = 10; // 에러가 발생하지 않음

  const add = (a, b) => {
    "use strict";
    let = 20; // SyntaxError: Unexpected strict mode reserved word
  };
  add(1, 2);
})();
```

이번에는 함수에 `use strict`를 쓴 함수와 쓰지 않은 함수가 공존하는 것이 바람직하지 않다고 한다...

따라서 strict mode는 `즉시 실행 함수`에서 사용하는 것을 권장한다.

```jsx
(function () {
  "use strict";
  // 함수 내용
});
```

</br>

### **20.5 strict mode가 발생시키는 에러**

**20.5.1 암묵적 전역**

선언하지 않는 변수 참조는 `ReferenceError` 발생

```jsx
(function () {
  "use strict";

  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

**20.5.2 변수, 함수, 매개변수의 삭제**

**변수**, **함수**, **매개변수**를 `delete`로 삭제하면 `SyntaxError` 발생

```jsx
(function () {
  "use strict";

  // variable
  var x = 1;
  delete x; // SyntaxError: Delete of ...

  // argument
  function foo(a) {
    delete a; // SyntaxError: Delete of ...
  }

  // function
  delete foo; // SyntaxError: Delete of ...
})();
```

**20.5.3 매개변수 이름의 중복**

중복된 매개변수 이름은 `SyntaxError` 발생

```jsx
(function () {
  "use strict";

  function add(x, x) {
    return x + x;
  }
})(); // SyntaxError: Duplicate parameter name not ...

(function () {
  // 화살표 함수는 엄격모드 안해도 에러를 낸다.
  const add = (x, x) => {
    return x + x;
  };
})(); // SyntaxError: Duplicate parameter name not ...
```

**20.5.4 with 문의 사용**

`with`를 사용하면 무조건 `SyntaxError`..

`with`를 사용하면 코드는 간단해지지만, 성능과 가독성이 나빠진다.

```jsx
(function () {
  "use strict";

  // SyntaxError: Strict mode code may not include ...
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

<details>
<summary>with문이 뭔데?</summary>

**이건 왜 존재하는 것일까?**

```jsx
// Before
x = Math.cos(3 * Math.PI) + Math.sin(Math.LN10);
y = Math.tan(14 * Math.E);

// After
with (Math) {
  x = cos(3 * PI) + sin(LN10);
  y = tan(14 * E);
}
```

> 그냥 못본 척 하고 평생 몰라도 될 것 같습니다.

</details>

<br/>

<details>
<summary>ESLint</summary>

그냥 `ESLint` 씁시다.
책 쓴 사람도 린트 쓰라고 했었습니다.

> eslintrc가 없다는 것은, 린터를 사용하지 않는다는 말이며, 린터를 사용하지 않는다는 말은 입문 단계라 린터가 뭔지 존재도 모른다던가, 아니면 나는 코드를 아주 마이웨이로 짜기 때문에 팀플레이는 고려하지 않는 사람이라던가, 아니면 수십년간의 경험으로 손린터가 린터보다 더 대단한 경지에 올랐기 때문일 것이다. 키보드에 0과 1 버튼만 있는 사람이 아니라면 린터를 쓰는게 맞다. 린터를 쓰지 않는 것은, 코드를 아무 규칙 없이 개같이 짠다는 말이고, 린터를 쓰지 않는 것은, 진짜 근본도 없는 개쌍놈이라는 것을 뜻하며, 린터를 쓰지 않는 것은, .. 그만..  
> [- 제가 쓴게 아닙니다.](https://miriya.net/blog/cliz752zc000lwb86y5gtxstu)

</details>
