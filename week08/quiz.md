## Quiz

- 진행자 : 서범석
- 날짜 : Sep 26 2023 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래 코드는 어떤 결과를 출력하는가?

```jsx
const add = (a, b) => a + b;

(function (f) {
  "use strict";

  function javascript() {
    console.log(this); // 여기
  }
  javascript();

  const minus = (a, b) => a - b;
  f = minus;

  console.log(f(2, 1)); // 여기
  console.log(arguments); // 여기
})(add);
```

<!--
답: undefined, 1, {0: (a, b) => a + b, length: 1, ...}

1. strict mode에서 일반함수안의 this는 undefined로 정의되며 에러를 발생하지 않는다.
2. strict mode에서 변수 f에 minus 함수를 재할당하는 것은 가능하다.
3. strict mode에서 arguments 객체는 변화하지 않는다.
-->

</br>

> 2. 다음 코드에서 어떤 값이 출력되는지 말하고, 이유를 설명하라

```jsx
const obj = "{age: 25}";
obj.age = 24;

console.log(obj.age); // 여기랑
console.log(obj); // 여기
```

<!--
답: undefined, '{age: 25}'

obj는 문자열이다.
obj를 마침표 표기법으로 접근하면 (.age) obj는 String의 래퍼 객체를 가리킨다.
해당 래퍼 객체가 age라는 프로퍼티를 갖고, 이내 가비지 컬렉션에 의해 메모리에서 정리된다.
그 후 obj는 내부슬롯 [[StringData]]에 있는 값('{age: 25}')을 다시 가리키고, 해당 값에는 age가 없으므로 undefined를 출력한다.
-->

</br>

> 3. 아래 여러 함수의 출력 값을 말하고 이유를 설명하라

```jsx
isNaN(null);
isNaN(Infinity);
isNaN(new Date());

parseFloat(" 59 점은 과락 ");
parseFloat("아쉽다 1 점");
```

<!--
답: false, false, false, 59, NaN

1. null -> 0 = Number
2. Infinity = Number
3. new Date() = Number
4. 앞 뒤 공백 제거, 맨 앞의 59만 변환
5. 맨 앞이 숫자로 판단할 수 없으면 NaN
-->

</br>

> 4. 아래 여러 함수의 출력 값을 말하고 이유를 설명하라

```jsx
parseInt(10);

parseInt("0xf");
parseInt("0b10");
parseInt("583", 8);
```

<!--
답: 10, 15, 0, 5

1. 10은 문자열 '10'으로 변환 후 해석하여 10으로 다시 변환
2. 16진수 'f'를 10진법으로 변환
3. 0b10, 2진수와 8진수는 오류로 인해 맨 앞의 0만 출력
4. 8진수에서 읽을 수 없는 8의 뒤는 무시
-->

</br>

> 5. `encodeURI`와 `encodeURIComponent`의 차이점은?

<!--
답: 인수로 전달된 문자열을 encodeURI는 전체 URI로 보고, ~Component는 쿼리스트링의 일부로 본다.
따라서 쿼리 스트링 구분자인 ?, &, =를 encodeURIComponent는 인코딩하지만, encodeURI는 인코딩하지 않는다.
-->

</br>

> 6. 출력 결과를 말하고 이유를 말하라

```jsx
console.log(x);
console.log(y);

var x = 10;

function f() {
  y = 20;
}
f();

console.log(x + y);
```

<!--
답: undefined, referenceError: y is not defined, 30

1. 전역변수 x는 변수 호이스팅이 발생한다.
2. 암묵적 전역에 의해 전역변수의 프로퍼티가 되는 y는 변수가 아니기에 변수 호이스팅이 발생하지 않는다.
3. 암묵적 전역에 의해 x와 y를 전역변수를 생략하고 참조할 수 있다.
-->
