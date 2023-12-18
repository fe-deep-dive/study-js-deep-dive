## Quiz

- 진행자 : 박지영
- 날짜 : Dec 18 2023

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 다음 코드의 실행 결과를 말하시오.

```jsx
const set = new Set();

set.add(1).add(2).add('hello');
console.log(set); // 1

set.size = 10;
console.log(set.size); // 2

console.log(set.has('h')); // 3

console.log(set.delete(3)); // 4
set.delete(1).delete(2); // 5
```

```
console.log(set); // 6
```

<!--
답:
1. Set(3) {1, 2, 'hello'}
2. 3
3. false
4. false (존재하지 않는 요소를 삭제하면 에러없이 무시된다.)
5. TypeError: set.delete(...).delete is not a function
6. Set(2) {2, 'hello'}
-->

<br>

> 2. 다음 리턴문에 들어갈 한 줄짜리 코드를 작성하시오.

```jsx
Set.prototype.func = function (set) {
  return ; // 여기
};

const setA = new Set('믿음직힌ssafy');
const setB = new Set('음직');

console.log(setA.func(setB)); // Set(6) {'믿', '힌', 's', 'a', 'f', 'y'};
```

<!--
답:
차집합을 구하는 함수이다.
new Set([...this].filter(v => !set.has(v)))
-->
