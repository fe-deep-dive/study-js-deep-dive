## Quiz

- 진행자 : 김민정
- 날짜 : Sep 12 2023

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 다음 코드의 출력 결과를 설명하고 변수 a, b에 할당되는 값과 코드 실행과정의 차이를 말하라.

```jsx
function Square(length) {
  this.length = length;
  this.getArea = function () {
    return this.length ** 2;
  };
}

const a = Square(4); // A
const b = new Square(5); // B

console.log(a.length, a.getArea()); // 1
console.log(b.length, b.getArea()); // 2
console.log(length, getArea()); // 3
```

<!--
1 : Uncaught TypeError: Cannot read properties of undefined (reading 'length')
2 : 5 25
3 : 4 16

A
new 없이 호출하였으므로 일반 함수로서 동작한다.
이때 this가 가리키는 값 (this 바인딩)은 전역 객체이다.
따라서 length 와 getArea는 전역 객체의 프로퍼티와 메서드로서 동작한다.
Square 함수의 내부 메서드인 [[Call]]이 실행된 것으로 볼 수 있다.

B
new와 함께 호출하였으므로 생성자 함수로 동작한다.
생성자 함수는 암묵적으로 인스턴스를 생성하고, 생성된 인스턴스를 초기화하고 프로퍼티를 추가하고 초기값을 할당한다.
이후 암묵적으로 인스턴스를 반환하여 반환된 객체(인스턴스)가 b에 저장된다.
이때 this는 생성자 함수가 미래에 생성할 인스턴스를 가리킨다.
Sqaure 함수의 내부 메서드인 [[Construct]]가 실행된 것으로 볼 수 있다.
-->

<br />
<br />

> 2. 다음 코드는 TypeError를 발생시킨다. TypeError가 발생하는 이유를 말하라.

```jsx
const Book = (title) => {
  this.title = title;
};

const book = new Book('모던 자바스크립트 Deep Dive');
```

<!--
자바스크립트 엔진은 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다.
화살표 함수와 메서드(ES6 축약표현에 한함)는 non-constructor로서 내부 메서드인 [[Construct]]가 존재하지 않는다.

코드에서 new 연산자와 함께 Book 함수를 호출하였으므로 생성자 함수로서 동작해야 하지만, [[Construct]]가 없으므로 TypeError가 발생한다.

Uncaught TypeError: Book is not a constructor
 -->

<br />
<br />

> 3. `이것`은 ES6에서 도입된 것으로 `new` 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 사용할 수 있도록 오류를 방지하는데 사용한다.  
>    다음 빈칸에 들어갈 `이것`과, `이것`이 도입되기 전에는 어떤 방법을 통해 생성자 함수를 작성했는지 말하라.

```jsx
function Human(name) {
  if (!'이것') {
    // Here
    return new Human(name);
  }

  this.name = name;
  this.sayHello = function () {
    console.log('Hello, my name is ' + this.name);
  };
}
```

<!--
'이것' = new.target

new.target은 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고도 한다.
함수 내부에서 new.target을 사용하여 new 연산자와 함께 생성자 함수로 사용되었는지 확인할 수 있다.

new.target의 도입 이전에는 스코프 세이프 생성자 패턴을 사용해 생성자 함수를 재귀적으로 호출했다.

'이것'
- ES6 : new.target
- ~ES5 : (this instanceof Human)
 -->

<br />
<br />

> 4.  다음 코드의 출력 결과와 왜 이런 차이가 일어나는지 말해라.

```jsx
const a = String(123);
const b = new String(456);

console.log(typeof a); // 1
console.log(typeof b); // 2
```

 <!-- 
 1: string (a = '123')
 2: object (b = String {"123"})

 String, Boolean, Number 생성자 함수는 new 연산자와 함께 호출하면 객체를 반환하지만, 없이 호출할 경우 각 타입 원시값을 반환한다.
 따라서 이를 통한 데이터타입 변환이 가능한다.
  -->

<br />
<br />

> 5. 일급 객체의 조건 4가지를 말해라.

<!--
1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.
 -->

 <br />
 <br />

> 6.  다음 코드의 실행 결과와 그 이유를 말해라.

```jsx
const javascript = { is: 'weird' };

console.log(javascript.hasOwnProperty('is')); // 1
console.log(javascript.hasOwnProperty('__proto__')); // 2
console.log(javascript.__proto__ === Object.prototype); // 3

// bonus
const js = new String('close ear');
console.log(js.__proto__);
```

<!--
javascript는 객체 리터럴로 생성한 객체이다.
javascript 객체는 [[Prototype]] 내부 슬롯을 가지며, [[Prototype]]이 가리키는 프로토타입 객체에 접근하기 위해서는 __proto__ 접근자 프로퍼티를 사용해야 한다.

hasOwnProperty 메서드는 Object.prototype의 메서드로서 객체 고유의 프로퍼티 키여야만 true를 반환한다.

javascript 객체의 __proto__ 접근자 프로퍼티를 통해 javascript 객체의 [[Prototype]]인 Object.prototype에 접근할 수 있다.

1: true
2: false
3. true

bonus: String {'', constructor: ƒ, anchor: ƒ, at: ƒ, big: ƒ, …}
 -->
