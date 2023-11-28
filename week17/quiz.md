## Quiz

- 진행자 : 박진아
- 날짜 : Nov 28 2023 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래 출력 결과를 설명하세요

```jsx
const mySymbol1 = Symbol.for('心볼');
const mySymbol2 = Symbol('心볼');
const mySymbol3 = Symbol.for('心볼');

console.log(mySymbol1 === mySymbol2);
console.log(mySymbol1 === mySymbol3);
```

<!--
false
true

Symbol 함수를 호출해 생성한 심벌 값은 전역 심벌 레지스토리에 등록되어 관리되지 않아
console.log(mySymbol1 === mySymbol2)는 false가 된다.

Symbol.for는 인수로 전달받은 문자열을 키로 사용해 전역 심벌 레지스토리에서 해당 키와 일치하는 심벌 값을 검색한다.
검색 성공시 검색된 심벌 값을 반환하므로 mySymbol1 === mySymbol3는 true를 반환한다.
-->

</br>

> 2. 출력 결과는 무엇이고 어떻게 개선하면 좋을까요?
```jsx
const arrayLike = {
	0: 1,
	1: 2,
	2: 3,
	length: 3
};

for (const item of arrayLike){
	console.log(item);
}
```

<!--
유사배열 객체는 이터러블 객체가 아니므로 for...of로 순회할 수 없어 TypeError: arrayLike is not iterable 가 발생한다.
Array.from()을 사용해 배열로 만들어 이터러블 객체로 만들면 된다.
-->

> 3. 다음 출력 결과를 설명하세요
```jsx
const conter = function(){
  const max = 3;
  let cur = 0;
  return {
    [Symbol.iterator](){ return this; },
    next(){
      cur += 1;
      return { value: cur, done: cur >= max };
    }
  }
}

f1 = conter();

f1.next()
f1.next()
f1.next()
f1.next()
```

<!--
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: true }
{ value: 4, done: true }

next는 한단계씩 순회하므로 value는 계속 증감한다.
done은 cur >= max를 만족하는 때에 true가 된다.
-->
