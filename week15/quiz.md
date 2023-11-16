## Quiz
- 진행자 : 박진아
- 날짜 : Nov 16 2023
---
> 1. 출력결과는 뭘까요? 그리고 둘의 차이점은 무엇일까요? 

```jsx
const foo = new Number('0');
const bar = Number('0'); 

console.log(typeof foo);
console.log(typoeof bar);
```

<!-- console.log(typeof foo); // object
console.log(typoeof bar); // number 
이 출력된다.

foo는 Number 인스턴스를 반환
bar: 숫자를 반환 -->


> 2. 다음 메서드를 간단하게 설명해주세요

```jsx
Math.abs
Math.round
Math.ceil
Math.floor
Math.sqrt
Math.random
Math.pow
Math.max
Math.min
```

<!--
Math.abs: 절댓값 반환
Math.round: 소수점 이하 반올림한 정수
Math.ceil: 소수점 이하 올림한 정수
Math.floor: 소수점 이하를 내림한 정수
Math.sqrt: 제곱근 반환
Math.random: 0에서 1미만의 실수 반환
Math.pow: 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곡
Math.max: 전달받은 인수 중 가장 큰 수 반환, 인수가 전달되지 않으면 -Infinity
Math.min: 전달받은 인수 중 가장 작은 수 반환, 인수가 전달되지 않으면 Infinity
-->

> 3. 다음 결과값은 무엇일까요? 왜 그렇게 생각했나요?
```jsx
Number.MAX_VALUE > Math.max(); // ?
```
<!--
ture.
Math.max()에서 아무 인수를 전달하지 않으면 -Infinity가 되므로 Number.MAX_VALUE값이 더 크다.
-->

> 4. 1에서 10범위의 랜덤 정수를 취득하는 코드를 짜보세요
<!--
```jsx
const random = Math.floor((Math.random()*10) +1);
```
-->

> 5. 어떻게 출력 될까요?
```jsx
const today = new Date();
console.log(today.getFullYear(), today.getMonth(), today.getDate());
```

<!--
2023 10 16
-->

