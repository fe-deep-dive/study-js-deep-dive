## 22장: this

---

### **22.1 this 키워드**

객체 지향 프로그래밍에서 메서드는 자신이 속한 프로퍼티를 참조, 즉 **자신이 속한 객체를 가리키는 식별자**를 참조할 수 있어야 한다.

```jsx
const js = {
  // property
  weird: 1,
  // method
  isWeird() {
    // 자신이 속한 js 재귀적으로 참조
    return js.weird != 0 ? true : false;
  },
};
console.log(js.isWeird()); // true
```

`isWeird` 메서드 안에서 자신이 속한 객체를 가리키는 식별자 `js`를 참조하고 있다.  
해당 참조표현식이 평가되는 시점은 `isWeird` 메서드가 호출되는 시점이다.

하지만 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지도, 바람직하지도 않다.

```jsx
function Javascript(isLanguage) {
  ????.isLanguage = isLanguage
}
```

생성자 함수의 객체 생성 방식은 `new` 연산자로 생성자 함수를 호출해야 한다.  
생성자 함수를 정의하는 시점에는 아직 인스턴스 생성 전이므로 자신이 생성할 인스턴스를 알 수 없다.

이를 위해 `this` 식별자를 제공한다.

### this

- **자신이 속한 객체 또는 자신이 생성할 인스턴스**를 가리키는 자기 참조 변수<sup>self-referencing variable</sup>

- 자바스크립트 엔진에 의해 **암묵적으로** 생성되며, 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달된다.

- `this`가 가리키는 값, **this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

<details>
<summary>this 바인딩</summary>

- 바인딩 : 식별자와 값을 연결하는 과정

- this 바인딩 : this와 this가 가리킬 객체를 바인딩
</details>

</br>

```jsx
// 객체 리터럴
const javascript = {
  weird: 1,
  isWeird() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return this.weird != 0 ? true : false;
  },
};

// 생성자 함수
function Javascript(isLanguage) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.isLanguage = isLanguage;
}
// 인스턴스 생성
const js = new Javascript(false);
```

객체 리터럴의 메서드에서 `this`는 호출한 객체, `javascript`를 가리킨다.

생성자 함수에서 `this`는 생성할 인스턴스, `js`를 가리킨다.

이처럼 **this는 상황에 따라 가리키는 대상이 다르다.**

또한 `this`는 코드 어디에서든 참조 가능하다.

```jsx
// 전역에서 this는 전역 객체를 가리킨다.
console.log(this); // window

function isLanguage(lang) {
  // 일반 함수 내부에서 this는 전역 객체를 가리킨다.
  console.log(this); // window
  return lang === "js" ? false : true;
}
isLanguage("js");

const javascript = {
  weird: true,
  isWeird() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {weird: true, isWeird: ƒ}
    return this.weird;
  },
};
console.log(javascript.isWeird()); // true

function Javascript(isWeird) {
  this.isWeird = isWeird;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Javascript {isWeird: true}
}
const js = new Javascript(true);
```

</br>

### **22.2 함수 호출 방식과 this 바인딩**

`this 바인딩`은 **함수 호출 방식에 따라 동적으로 결정된다.**

<details>
<summary>렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.</summary>

- 렉시컬 스코프<sup>lexical scope</sup> : 함수 정의가 평가되어 함수 객체가 생성되는 시점
- this 바인딩 : 함수 호출 시점
</details>

</br>

함수 호출 방식은 크게 4가지가 있다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출

**this에는 전역 객체<sup>global object</sup>가 바인딩된다.**

```jsx
function foo() {
  console.log(this); // window
  function bar() {
    console.log(this); // window
  }
  bar();
}
foo();
```

**scrict mode**가 적용된 일반 함수 내부에서의 `this`는 `undefined`가 바인딩된다.

메서드 내에서 정의한 중첩 함수 내부의 `this`에는 전역 객체가 바인딩된다.

```jsx
var val = "hey";
// const로 선언하면 전역 객체의 프로퍼티가 아니다.

const obj = {
  val: "halo",
  foo() {
    console.log(this); // {val: 'halo', foo: ƒ}
    console.log(this.val); // 'halo'

    // 중첩 함수
    function bar() {
      console.log(this); // window
      console.log(this.val); // 'hey'
    }
    bar();
  },
};

obj.foo();
```

**어떤 함수라도 (콜백 함수 포함) 일반 함수로 호출**되면 `this`에 전역 객체가 바인딩된다.

```jsx
var val = "hey";

const obj = {
  val: "halo",
  foo() {
    console.log(this); // {val: 'halo', foo: ƒ}
    // 콜백 함수(setTimeout) 내부의 this: 전역 객체 바인딩
    setTimeout(function () {
      console.log(this); // window
      console.log(this.val); // 'hey'
    }, 100);
  },
};
```

이는 중첩 함수 또는 콜백 함수를 헬퍼 함수로 동작하기 어렵게 만든다.

```jsx
// 중첩, 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법
var val = "hey";

const obj = {
  val: "halo",
  foo() {
    // 1. this 바인딩을 변수 that에 할당
    const that = this;

    // 콜백 함수 내부에서 this 대신 that 참조
    setTimeout(function () {
      console.log(that.val); // 'halo'
    }, 100);

    // 2. 화살표 함수 사용, 화살표 함수의 this는 상위 스코프 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 'halo'
  },
};

obj.foo();
```

### 22.2.2 메서드 호출

메서드 내부의 `this`에는 메서드를 호출한 객체가 바인딩된다.

```jsx
const javascript = {
  weird: true,
  isWeird() {
    return this.weird;
  },
};

console.log(javascript.isWeird()); // true
```

`isWeird()` 메서드는 `javascript` 객체의 메서드로 정의되었다.

`isWeird` 프로퍼티가 가리키는 함수 객체는 `javascript` 객체에 포함된 것이 아니라 독립적으로 존재하는 객체다.

> isWeird() 라는 '함수 객체'가 갖고 있는 메모리 공간을 isWeird 프로퍼티가 참조한다는 뜻

따라서 `isWeird()` 메서드는 다른 객체의 메서드가 될 수도, 일반 함수로 호출될 수도 있다.

```jsx
const java = {
  weird: false,
};

// isWeird 메서드를 java 객체의 메서드로 할당
java.isWeird = javascript.isWeird;
console.log(java.isWeird()); // false

// 메서드를 변수에 할당
const isWeird = javascript.isWeird;
console.log(isWeird()); // undefined
// 일반 함수로 호출하면 window.isWeird()와 같다
```

<details>
<summary>책에선 ''이 출력되던데?</summary>

예제 22-15에서 마지막 `getName()`은 `window.name`을 출력하여 `''`이 출력된다.

다른 예제를 생각하며 입력하면 `undefined`가 출력된다.

이는 window 객체가 `name` 프로퍼티를 `''`로 갖고 있기 때문에 `''`이 출력된다.

</details>

</br>

프로토타입 메서드 내부의 `this`도 해당 메서드를 호출한 객체에 바인딩된다.

```jsx
function Language(weird) {
  this.weird = weird;
}

Language.prototype.isWeird = function () {
  return this.weird;
};

const javascript = new Language("Javascript is not a Language");
console.log(javascript.isWeird()); // 1. Javascript is not a Language

Language.prototype.weird = "Let's use Typescript";
console.log(Language.prototype.isWeird()); // 2. Let's use Typescript
```

1. 1의 경우 `isWeird` 메서드를 호출한 객체는 `javascript`

2. 2의 경우 `isWeird` 메서드를 호출한 객체는 `Language.prototype`

### 22.2.3 생성자 함수 호출

생성자 함수 내부의 `this`에는 **생성자 함수가 생성할 인스턴스**가 바인딩된다.

```jsx
function Language(name) {
  this.name = name;
  this.isLang = function () {
    return name === "javascript" ? false : true;
  };
}

const js = new Language("javascript");
const java = new Language("java");

console.log(js.isLang()); // false
console.log(java.isLang()); // true

// new 연산자를 사용하지 않으면 일반 함수로 호출된다.
const python = Language("python");
console.log(python); // undefined
// 전역 객체, 즉 window 객체의 name이 바뀐다.
console.log(name); // python
```

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

`Function.prototype`의 메서드인 `apply`, `call`, `bind` 메서드는 모든 함수가 상속받아 사용 가능하다.

`apply`와 `call` 메서드는 `this`로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

```jsx
function foo() {
  return this;
}
console.log(foo()); // window

const thisObj = { a: 1 };
console.log(foo.apply(thisObj)); // {a: 1}
console.log(foo.call(thisObj)); // {a: 1}
```

`apply`와 `call` 메서드는 **함수를 호출**한다.

두 메서드의 차이점은 아래 예시를 보자.

```jsx
function func() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisObj = { js: "not lang" };

console.log(func.apply(thisObj, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {js: 'not lang'}

console.log(func.call(thisObj, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {js: 'not lang'}
```

위의 예제처럼 `apply`와 `call`은 사용 방식에만 차이가 있을 뿐, 결과는 같다.

`apply`는 호출할 함수의 인수를 **배열로 묶어** 전달한다.

`call`은 호출할 함수의 인수를 **쉼표로 구분한 리스트 형식**으로 전달한다.

</br>

**그렇다면 `apply`나 `call`은 왜 쓰는걸까?**

`arguments`객체는 유사 배열 객체, 즉 배열이 아니라서 `Array.prototype.slice`같은 배열 세머드를 사용할 수 없다.

하지만 `apply`와 `call` 메서드를 이용하면 가능하다.

```jsx
function argsToArr() {
  const arr = Array.prototype.slice.call(arguments);
  return arr;
}

argsToArr(1, 2, 3); // [1, 2, 3]
```

이런게 된다고 한다.

`Function.prototype.bind` 메서드는 `apply`, `call`과 다르게 함수를 호출하지 않는다.

`bind` 메서드는 **this 바인딩이 교체된 함수를 새롭게 생성해 반환**한다.

```jsx
function func() {
  return this;
}

const thisObj = { a: 1 };
console.log(func.bind(thisObj)); // func
console.log(func.bind(thisObj)()); // {a: 1}
```

`bind` 메서드는 주로 `this`가 불일치하는 문제를 해결하기 위해 사용된다.

```jsx
const javascript = {
  weird: "weird",
  isWeird(callback) {
    setTimeout(callback, 100);
  },
};

javascript.isWeird(function () {
  console.log(`Javascipt is ${this.weird}.`); // Javascript is undefined.
});
```

위 예제에서 `javascript.isWeird`의 콜백 함수 `callback`은 외부함수를 돕는 역할을 한다.

이 때 외부함수 `isWeird` 내부의 `this`와 콜백함수 내부의 `this`가 다르므로 문제가 발생한다.

이를 `bind`를 사용하여 `this`를 일치시킬 수 있다.

```jsx
const javascript = {
  weird: "weird",
  isWeird(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

javascript.isWeird(function () {
  console.log(`Javascipt is ${this.weird}.`); // Javascript is weird.
});
```
