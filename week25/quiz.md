## Quiz

- 진행자 : 박진아
- 날짜 : Feb 13 2024 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래 코드의 수행 결과를 설명해주세요.

```jsx
function* 올해목표(){
  yield '싸';
  yield '탈';
  yield '취';
  yield '뽀';
  yield '기';
  yield '원';
}

const gen = 올해목표();
gen.next();
gen.next();
gen.next();
gen.next();

for(const x of gen){
  console.log(x);
}

```

<!--
순서대로 싸탈취뽀기원.
제너레이터 객체는 이터레이터이자 이터러블이다.
-->

</br>

> 2. 다음의 수행 결과는 어떻게 되는지 설명해주세요.

```jsx
function* genFunc() {
  const x = yield 1;

  const y = yield x + 10;

  return x + y;
}

const generator = genFunc(0);

let res = generator.next();
console.log(res); 

res = generator.next(10);
console.log(res); 

res = generator.next(40);
console.log(res);
```

<!--
답: 
처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
{ value: 1, done: false }

next 메서드에 인수를 전달한 10은 genFunc 함수의 x 변수에 할당된다.
이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
{ value: 20, done: false }

genFunc 함수의 y 변수에 할당 된다.
이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 50이 할당 된다.
{ value: 50, done: true }
-->
