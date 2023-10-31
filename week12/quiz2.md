## Quiz

- 진행자 : 김민정
- 날짜 : Oct 24 2023

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

<br />

> 1. 다음 코드의 실행 결과와 실행 과정을 말하라.

```js
class Person {
  #state = '';

  constructor(state) {
    this.#state = state;
  }

  get state() {
    return this.#state;
  }
}

const me = new Person('sleepy');
console.log(me.state);
```

<!--
#state는 private 필드로 클래스 몸체에서 클래스 필드로 선언되었다.
constructor에서 인수 state를 통해 초기화 되었다.
다만 접근자 프로퍼티인 state에 의해 public처럼 접근이 가능해져 console.log(me.state)에서 접근하여 'sleepy'가 출력된다.
-->

<br />

> 2. 클래스 상속 시 서브클래스에서 super를 먼저 호출해야 하는 이유에 대해 설명하라.

```js
class Base {}

class Derived extends Base {
  constructor() {} // Error

  constructor() {
    this.a = 1; // Error
    super();
  }
}
```

<!--
클래스는 new 연산자와 함께 호출되면 암묵적으로 빈 객체 = 인스턴스를 생성하고 this에 바인딩한다.
하지만 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 이를 위임한다.
따라서 super 호출을 통해 수퍼클래스의 constructor를 실행하고 super가 반환한 인스턴스를 this에 바인딩 해야만 this를 참조할 수 있다.
-->

<br />

> 3. `[[HomeObject]]`에 대해서 설명하고, 다음 코드에서 super의 바인딩 과정을 설명하라.

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  sayHi() {
    return `${super.sayHi()}. How are you?`;
  }
}

const derived = new Derived('손흥민');
console.log(derived.sayHi());
```

<!--

Derived 클래스의 sayHi 메서드 - Derived.prototype에 바인딩
Derived 클래스의 sayHi 메서드의 [[HomeObject]] - Derived.prototype
Derived 클래스의 sayHi 메서드 내부의 super 참조 - Base.prototype
super.sayHi -  Base.prototype.sayHi

-->
