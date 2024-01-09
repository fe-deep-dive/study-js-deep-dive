## Quiz

- 진행자 : 서범석
- 날짜 : Jan 9 2024 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래의 코드는 어떻게 동작하는지 설명하고, `DOMContentLoaded`와 `load`의 차이는 무엇인지 설명해주세요.

```jsx
window.addEventListener("DOMContentLoaded", () => {
  console.log("DOMContentLoaded");
});

window.addEventListener("load", () => {
  console.log("load");
});
```

<!--
답: DOMContentLoaded -> load 순으로 출력

load는 모든 리소스(CSS, images, ...)가 다운로드된 다음 이벤트 호출
DOMContentLoaded는 HTML document를 전부 읽고 DOM트리를 완성하는 즉시 이벤트가 호출된다.
-->

<br>

> 2. 이벤트 버블링과 캡처링은 어떻게 다른지 설명해주고, 어떤 값이 기본값이고 기본값이 아닌 값은 어떻게 사용하는지 설명해주세요.

<!--
답:
이벤트 버블링: 한 요소에 이벤트가 발생하면 최상단의 부모 요소를 만날 때까지 이벤트 핸들러가 동작

이벤트 캡처링: 버블링과 반대로, 최상위 태그에서부터 해당 태그를 찾아 내려감
-->
