## Quiz
- 진행자 : 서범석
- 날짜 : Aug 8 2023
---
<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 변수, 함수 이름 등 식별자가 등록되는 곳은 어디이며, 어떤 형식으로 관리되는가?

<!--
답: 실행 컨텍스트, 변수 이름(key) / 변수 값(value) 형식
-->
</br>

> 2. 첫 코드의 `x`는 호이스팅되어 `undefined`로 출력된다. 그렇다면 아래의 코드는 어떻게 동작할까?

```jsx
var x = 'outer scope';
(function() {
  console.log(x); // undefined
  var x = 'inner scope';
}());
```

```jsx
const x = 'outer scope';
(function() {
  console.log(x);
  const x = 'inner scope';
}());
```

<!--
답: referenceError 발생
함수 안의 const x가 호이스팅되기 때문에 'outer scope'로 인식하지 않고 에러를 발생시킨다. (참고: TDZ)
-->
</br>

> 3. 아래의 코드에서 메모리 공간은 몇 번 바뀌는가?

```jsx
var variable = 0;
variable = 1;
```

<!--
답: 3번
1. undefined, 2. 0, 3. 1
-->
</br>

> 4. 아래의 함수는 무엇을 리턴하는가? 이유는?

```jsx
function foo(a) {
  if (a) return
  a *= 2;
}

foo(2)
```

<!--
답: undefined
ASI에 의해 return의 끝에 세미콜론이 찍히기 때문이다.
-->
</br>

> 5. 해당 코드는 무엇을 출력하는가?
```jsx
const x = 10 / 0;
const y = 10 / -0;
console.log(x - y);
```

<!--
답: Infinity
Infinity - (-Infinity) = Infinity
x * y = -Infinity
x / y = NaN
x + y = NaN
-->

> 6. 좋은 코드는 무엇이라고 생각하시나요?

<!--
답: 정해진 답이 없다.
-->
