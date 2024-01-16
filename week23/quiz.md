## Quiz

- 진행자 : 박지영
- 날짜 : Jan 16 2024 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래의 코드의 출력 결과를 말하세요.

```jsx
function sleep(func, delay) {
  const delayUntil = Date.now() + delay;

  while (Date.now() < delayUntil);

  func();
}

function fu() {
  console.log('fu');
}

function ba() {
  console.log('ba');
}

function o() {
  console.log('o');
}

sleep(fu, 3 * 1000);
setTimeout(ba, 3 * 1000);
o();
```

<!--
답:
fu
o
ba
-->

<br>

> 2. XMLHttpRequest의 프로퍼티와 메서드에 대한 설명을 알맞게 짝지으세요.

```text
1. XMLHttpRequeest 객체의 프로토타입 프로퍼티 readyState
2. XMLHttpRequeest 객체의 프로토타입 프로퍼티 status
3. XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티 onreadystatechange
4. XMLHttpRequest 객체의 메서드 open
5. XMLHttpRequest 객체의 메서드 send
```

```text
a. readyState 프로퍼티 값이 변경된 경우
b. HTTP 요청 초기화
c. HTTP 요청의 현재 상태를 나타내는 정수
d. HTTP 요청에 대한 응답 상태를 나타내는 정수
e. HTTP 요청 전송
```

<!--
답:
1-c
2-d
3-a
4-b
5-e
-->
