## Quiz

- 진행자 : 서범석
- 날짜 : Jan 30 2024 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래 코드의 수행 결과와 시간을 말하고 설명해주세요.

```jsx
const all1 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));
const all2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 3000));
const all3 = () => new Promise((resolve) => setTimeout(() => resolve(1), 2000));

// 여기
Promise.all([all1(), all2(), all3()]).then(console.log).catch(console.error);

Promise.race([
  new Promise((_, reject) => setTimeout(() => reject(new Error(1)), 3000)),
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
  new Promise((resolve) => setTimeout(() => resolve(3), 2000)),
])
  .then(console.log) // 여기
  .catch(console.log);
```

<!--
답: 3.n초 후 [3, 2, 1], 2초 후 2

1. promise 배열을 모두 병렬적으로 실행, 가장 오래 걸린 약 3초 후 실행 순서가 보장된 순서로 return
2. 가장 먼저 끝나는 2를 return
-->

</br>

> 2. 다음의 수행 결과는 어떻게 되는지, fetch 함수는 어떤 값을 리턴하는지 설명해주세요.

```jsx
// valid url로 API를 호출하면 { id: 1, content: 'use Typescript' }를 돌려준다.
const data = fetch(`${validUrl}`, {
  method: "GET",
})
  .then((res) => res.json())
  .then((res) => res);

console.log(data);
```

<!--
답: undefined, Promise

비동기 함수는 Promise를 반환, 아직 응답이 오지 않았으므로 undefined가 출력된다.
-->
