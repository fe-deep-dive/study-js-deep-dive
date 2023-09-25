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
