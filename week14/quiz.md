## Quiz

- 진행자 : 서범석
- 날짜 : Nov 7 2023 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 각 메서드를 설명하고, 원본 배열을 변경하는 메서드와 변경하지 않는 메서드를 나눠주세요.

```jsx
push();
pop();
unshift();
shift();
concat();
splice();
slice();
join();
reverse();
fill();
flat();
```

<!--
답:
push: 인수를 추가, 변경 ○
pop: 마지막 요소 제거 후 반환, 변경 ○
unshift: 앞에 인수를 추가, 변경 ○
shift: 맨 앞 요소 제거 후 반환, 변경 ○
concat: 전달된 값을 합쳐 새 배열로 반환, 변경 ✕
splice: 배열의 중간에 요소를 추가 혹은 제거, 변경 ○
slice: 전달된 범위만큼 복사, 변경 ✕
join: 인수를 구분자로 연결해 문자열 반환, 변경 ✕
reverse: 거꾸로, 변경 ○
fill: 전달받은 요소로 범위만큼 채운다. 변경 ○
flat: 배열을 평탄화한다. 변경 ✕
-->

<br>

> 2. 다음의 출력 결과와 이유를 설명해주세요.

```jsx
const arr = [1, 2, , 4];
arr.hey = "h";
arr["hoi"] = "ho";
arr[-1] = -1;
arr[100] = 100;

arr.map((x) => {
  console.log(x);
  return x * 2;
});

console.log(arr);
```

<!--
답: [2, 4, empty, 8, empty ✕ 96, 200]
[1, 2, empty, 4, empty ✕ 96, 100, hey: 'h', hoi: 'ho', -1: -1]
-->

<br>

> 3. 다음 배열을 want 프로퍼티에 따라 내림차순으로 정렬해주세요.

```jsx
const wantToGo = [
  { want: Infinity, name: "Toss" },
  { want: 0.0001, name: "무직" },
  { want: 10, name: "Tmax" },
  { want: 100000000, name: "Carrot" },
];

const compare = (key) => {
  return (a, b) => ( ~~~ ? 1 : ~~~ ? -1 : 0)
};

// 어떤 함수를 어떻게 써야하나요?
```

<!--
답: a[key] > b[key], a[key] < b[key]
wantToGo.sort(compare('want)).reverse();
-->
