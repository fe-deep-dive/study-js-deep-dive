# 46장 제너레이터와 async await

## 46.1 제너레이터란?

제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개가능한 특수한 함수

특징

- 함수 호출자에게 함수 실행의 제어권을 양도가능: 함수 호출자가 함수 실행을 일시 중지시키거나 재개 할 수 있음
- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있음: 제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고 받을 수 있음
- 제너레이터 함수를 호출하면 제너레이터 객체를 반환: 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환함

## 46.2 제너레이터 함수의 정의

function\* 키워드로 선언하고 하나 이상의 yield 표현식을 포함

```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obh = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genObjMethod() {
    yield 1;
  }
}
```

애스터리크스(\*) 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없음. 하지만 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장함.

제너레이터 함수는 화살표 함수로 정의 불가.

```jsx
const genArrowFunc = * () => {
    yeield 1;
}; // SyntaxError: Unexpected token '*'
```

제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출 불가.

```jsx
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

## 46.3 제너레이터 객체

제너레이터 함수를 호출하면 제너레이터 객체를 생성해 반환함. 이 객체는 이터러블이면서 동시에 이터레이터 임.

<details>
    <summary>제너레이터 객체</summary>
    - 이터러블: Symbol.iterator 메서드를 상속 받음

    - 이터레이터: value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유

</details>

```jsx
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 이터레이터.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // ture
console.log("next" in generator); // true
```

**제너레이터의 메서드**
제너레이터 객체는 next, return, throw 세가지 메서드를 가짐

- next 메서드: 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환 `{ value: yield된 값, done: false }`
- return 메서드: return 메서드 호출시 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환 `{ value: 인수, done: true }`

```jsx
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: "End!", done: true}
```

- throw 메서드: 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환 `{ value: undefined, done: true }`

```jsx
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value:1, done: false}
console.log(generator.throw("Error!")); // {value: undefined, done: true}
```

## 46.4 제너레이터의 일시 중지와 재개

yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개 할 수 있음.

제너레이터 객체의 next메서드를 호출시 제너레이터 함수의 코드 블록을 실행할 수 있음. 이때 yield 표현식까지만 실행함. yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환함.

```jsx
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

// 첫 번째 yeild 표현식까지 실행되고 일시 중지한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 두 번째 yeild 표현식까지 실행되고 일시 중지한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2이 할당된다.
console.log(generator.next()); // {value: 2, done: false}

// 세 번째 yeild 표현식까지 실행되고 일시 중지한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 2이 할당된다.
console.log(generator.next()); // {value: 3, done: false}

// 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// value 프로퍼티에는 제너레이터의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next()); // {value: undefined, done: true}
```

next메서드를 호출시 yield 표현식까지 실행되고 일시 중지 됨. 이때 함수의 제어권이 호출자로 양도 됨. 또다시 next 메서드를 호출시 함수 실행을 재개할 수 있음.

**next 메서드에 인수를 전달하기**

```jsx
function* genFunc() {
  const x = yield 1;

  const y = yield x + 10;

  return x + y;
}

const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수를 전달한 10은 genFunc 함수의 x 변수에 할당된다.
// 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
res = generator.next(10);
console.log(res); // {value 20, done: false}

// genFunc 함수의 y 변수에 할당 된다.
// 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당 된다.
res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

제너레이터는 next메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고 받을 수 있음. next 메서드로 yield 표현식까지 함수를 실행시켜 제너레이터 객체가 관리하는 상태 값을 꺼내올 수 있음. 인수를 전달해 제너레이터 객체에 상태를 밀어넣을 수 있음.

## 46.5 제너레이터의 활용

### 이터러블 구현

이터레이션 프로토콜을 준수한 것보다 간단히 이터러블을 구현할 수 있음.

```jsx
// 이터레이션 프로토콜을 준수하는 방법
const infinitiFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      retur {value: cur}
    }
  }
}());
```

```jsx
// 제너레이터 함수로 구현하는 방법
const infinitiFibonacci = (function () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());
```

### 비동기 처리

then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있음

```jsx
const fetch = requre("node-fetch");

const async = (generatorFunc) => {
  const generator = generatorFunc(); // (2)

  const onResolved = (arg) => {
    const result = generator.next(arg); // (5)

    return result.done
      ? result.value // (9)
      : result.value.then((res) => onResolved(res)); // (7)
  };

  return onResolved; // (3)
};


async(function* fetchTodo() { // (1)
  const url = `${BASE_URL}/todos/1`;

  const response = yield fetch(url); // (6)
  const todo = yield response.json(); // (8)
  console.log(todo);
})(); //(4)
```

1. fetchTodo를 호출해 제너레이터 객체 생성:
   - async 함수가 호출되고(1) 인수로 전달받은 제너레이터 함수 fetchTodo를 호출
   - 제너레이터 객체를 생성(2)하고 onResolved 함수를 반환(3) 
2. fetch(url) yield 하기:
   - next 메서드가 처음 호출(5)되고 fetch(url)까지 실행
   - 이터레이터 리절트 객체 `{ value: Promise { <pending> }, done: false }` 반환
   - Response 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출(7)
3. response.json() yield 하기
   - 인수로 전달된 Response 객체를 next 메서드에 인수로 전달하면서 두번째로 next 호출
   - 인수는 response.json()의 `response` 변수에 할당 됨
   - response.json()까지 실행
   - 이터레이터 리절트 객체 `{ value: Promise { <pending> }, done: false }` 반환
   - response 프로미스를 resolve한 todo 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출
4. todo 출력하기
   - 인수로 전달된 todo 객체를 next 메서드에 인수로 전달하면서 세번째로 next 호출
   - fetchTodo의 todo 변수에 할당되고 제너레이터 함수 fetchTodo가 끝까지 실행
5. 종료하기
   - 이터레이터 리절트 객체 `{ value: undefined, done: true }` 반환
   - undefined를 그대로 반환하고 처리 종료

<details>
  <summary>
  로그를 찍어봤습니다.
  </summary>
  <img width="1106" alt="스크린샷 2024-02-13 오전 12 33 38" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/6b367dff-3c36-4c4b-bb1d-aac7e474e6b6">    
</details>




## 46.6 async/await

ES8에서 더 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현한 async/await를 도입

- 프로미스 기반으로 동작
- then/catch/finally 후속 처리 메서드를 사용하지 않고 동기처럼 사용할 수 있음
- 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있음

```jsx
const fetch = require("node-fetch");

async function fetchTodo() {
  const url = `${BASE_URL}/todos/1`;

  const response = await fetch(url);
  const todo = await response.json();

  console.log(todo);
}
```

### async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 함.
- async 함수는 async 키워드를 사용해 정의하고 언제나 프로미스를 반환.
- async 함수는 암묵적으로 반환값을 resolve 하는 프로미스를 반환

```jsx
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) { return n };
}
obj.foo(4).then((v)=>console.log(v)); //4

// async 클래스 메서드
class MyClass{
  async bar(n) {return n};
}
const myClass = MyClass();
myClass.bar(5).then((v)=>console.log(5)); // 5

```

클래스의 constructor 메서드는 async 메서드가 될 수 없음. 클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환 함.

### await 키워드

await 키워드 프로미스가 settled 상태가 될 떄까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환함. await 키워드는 반드시 프로미스 앞에서 사용해야 함.

```jsx
async function fetchTodo() {
  const url = `${BASE_URL}/todos/1`;

  const response = await fetch(url); // (1)
  const { title } = await response.json();

  console.log(title); // 싸탈하기
}
```

(1)의 상태가 settled될 떄까지 대기하게 됨. 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당 함.

await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled상태가 되면 다시 재개함.

```jsx
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요
```

모든 프로미스에 await를 사용하는 것을 주의해야 함. 위 foo 함수는 종료될 떄까지 약 6초가 소요됨.

- 첫 번째 프로미스가 settled 상태가 될 떄까지 3초
- 두 번쨰 프로미스가 settled 상태가 될 때까지 2초
- 세 번쨰 프로미스가 settled 상태가 될 떄까지 1초

foo는 개별적으로 처리해도 무방하므로 다음처럼 처리하는게 더 좋음.

```jsx
async function foo() {
  const res = await Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 3초 소요
```

다음 bar는 앞선 비동기 처리의 결과를 가지고 다음 비동기 처리를 수행해야 하므로 모든 프로미스에 await 키워드를 사용해 순차적으로 처리해야 함.

```jsx
async function bar(n) {
  const a = await new Promise((resolve) => setTimeout(() => resolve(n), 3000));
  const b = await new Promise((resolve) =>
    setTimeout(() => resolve(a + 1), 2000)
  );
  const c = await new Promise((resolve) =>
    setTimeout(() => resolve(b + 1), 1000)
  );

  console.log([a, b, c]); // [1, 2, 3]
}

bar(1); // 약 6초 소요
```

### 에러 처리

async/await에서 에러 처리는 try...catch 문을 사용할 수 있음. 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있어 호출처가 명확해 try catch를 사용할 수 있음.

```jsx
async function fetchTodo() {
  const url = `${BASE_URL}/wrong/todos/1`;

  try {
    const response = await fetch(url);
    const { title } = await response.json();

    console.log(title);
  } catch (err) {
    console.error(err); // TypeError: failed to fetch
  }
}

fetchTodo();
```

위 예제의 fetchTodo 함수의 catch문은 HTTP 통신에서 발생한 네트워크 에러뿐 아니라 try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치함.

async 함수 내에서 catch 문을 사용해 에러를 처리하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환함. 따라서 catch 후속 처리 메서드를 사용해 에러를 캐치할 수도 있음.

```jsx
async function fetchTodo() {
  const url = `${BASE_URL}/wrong/todos/1`;

  const response = await fetch(url);
  const { title } = await response.json();
  return title;
}

fetchTodo().then(console.log).catch(console.error); // TypeError: Failed to fetch
```
