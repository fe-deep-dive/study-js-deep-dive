## 33.1 심벌이란?

ES6에서 도입된 7번째 **데이터 타입**으로 변경이 불가능한 **원시 타입**이다. 다른 값과 중복되지 않은 **유일무이한 값**이다.

이름 충돌이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

> 원시값: 문자열, 숫자, 불리언, undefined, null, Symbol, bigint
> 

## 33.2 심벌 값의 생성

**Symbol 함수** 

리터럴 표기법이 아닌 Symbol 함수를 호출해서 생성해야 한다.

이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 **유일무이한 값**이다.

```jsx
// Symbol 함수를 호출해 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySumbol); // Symbol();
```

new 연산자와 함께 호출하지 않는다.

```jsx
new Symbol(); // TypeError: Symbol is not a constructor
```

Symbole 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되고 심벌 값 생성에 어떠한 영향을 주지 않는다. 즉 설명이 같더라도 유일무이한 값이 생성된다. 

```jsx
// 설명이 같더라도 값은 유일무이한 값이다.
const mySymbol1 = Symbol('클이쓰마스');
const mySymbol2 = Symbol('클이쓰마스');

console.log(mySymbol1 === mySymbol2); // false
```

심벌 값도 문자열, 숫자, 불리언처럼 객체처럼 접근시 암묵적으로 래퍼 객체를 생성한다. 다음 예제의 description 프로퍼티와 toString 메서드는 Symbol.prototype의 프로퍼티이다.

```jsx
const mySymbol = Symbol('꾸리쓰마스');
console.log(mySymbol.description); // '꾸리쓰마스'
console.log(mySymbol.toString()); // 'Symbol(꾸리쓰마스)'
```

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.

```jsx
const mySymbol = Symbol();
console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a number
```

그런데 불리언 타입으로는 암묵적으로 변환된다. 이를 통해 if문 등에서 존재 확인이 가능하다.

```jsx
const mySymbol = Symbol();
if(mySymbol) console.log('mySymbol is not empty');
```

**Symbol.for / Symbol.keyFor 메서드**

**Symbol.for**

인수로 전달받은 문자열을 키로 사용해 전역 심벌 레지스토리에서 해당 키와 일치하는 심벌 값을 검색한다.

> 전역 심벌 레지스토리: 키와 심벌 값의 쌍들이 저장되어 있는 저장소
> 
- 검색 성공시 검색된 심벌 값을 반환한다.
- 검색 실패시 새로운 심벌 값을 생성해 인수로 전달된 키로 전역 심벌 레지스토리에 저장한 후, 생성된 심벌 값을 반환한다.

```jsx
// 전역 심벌 레지스토리에 '킹왕짱 졸려' 라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('킹왕짱 졸려');
// 전역 심벌 레지스토리에 '킹왕짱 졸려' 라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('킹왕짱 졸려');

console.log(s1===s2); // true
```

전역 심벌 레지스토리는 심벌 값을 검색할 수 있는 키를 지정할 수 없다. 그러나 Symbol.for을 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 **단 하나만 생성**하여 전역 심벌 레지스토리를 통해 공유한다.

**Symbol.keyFor**

전역 심벌 레지스토리에 저장된 심벌 값의 키를 추출할 수 있다.

```jsx
// 전역 심벌 레지스토리에 '킹왕짱 졸려'라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('킹왕짱 졸려');
Symbol.keyFor(s1); // '킹왕짱 졸려'

// Symbol 함수를 호출해 생성한 심벌 값은 전역 심벌 레지스토리에 등록되어 관리되지 않는다.
const s2 = Symbol('킹왕짱 졸려');
Symbol.keyFor(s2); // undefined

```

## 33.3 심벌과 상수

4방향을 나타내는 상수를 정의한다.

```jsx
const Direction = {
	UP: 1,
	DOWN: 2,
	LEFT: 3, 
	RIGHT: 4
}

const myDirection = Direction.UP;

if (myDirection === Direction.UP){
	console.log('붐업');
}
```

위 코드의 문제점:

- 상수 값이 변경될 수 잇다
- 다른 변수 값과 중복될 수 있다.

→ 심벌로 중복될 가능성이 없는 값으로 만들자

```jsx
const Direction = {
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'), 
	RIGHT: Symbol('right')
}

const myDirection = Direction.UP;

if (myDirection === Direction.UP){
	console.log('붐업');
}
```

- enum
    
    enum은 명명된 숫자 상수의 집합으로 열거형이라 부른다. 자바스크립트는 enum을 지원하지 않는다. (타입스크립트는 지원) 자바스크립트에서 enum을 흉내 내어 사용하려면 객체의 변경을 방지하기 위해 Obejct.freeze메서드와 심벌 값을 사용한다.
    
    ```jsx
    const Direction = Object.freeze({
    	UP: Symbol('up'),
    	DOWN: Symbol('down'),
    	LEFT: Symbol('left'), 
    	RIGHT: Symbol('right')
    });
    
    const myDirection = Direction.UP;
    
    if (myDirection === Direction.UP){
    	console.log('붐업');
    }
    ```
    

## 33.4 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있고 동적으로 생성할 수 있다. 

프로퍼티 키로 사용할때, 접근할 때 대괄호를 사용해야 한다.

```jsx
const obj = {
    [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 **절대** 충돌하지 않는다. 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에도 충돌할 위험이 없다.

## 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하면 for…in문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 이처럼 심벌 값을 프로퍼티 키로 사용하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```jsx
const obj = {
	[Symbol('mySymbol')]: 1
};

for(const key in obj){
	console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```jsx
const obj = {
	[Symbol('mySymbol')]: 1
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0]; 
console.log(obj[symbolKey1]); // 1
```

## 33.6 심벌과 표준 빌트인 객체 확장

표준 빌트인 객체 확장을 권장하지 않는다.

이유:

- 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문이다.
- ES6가 도입되기전 Array.prototype.find를 추가했다면 ES6의 find 메서드는 사용자 정의 find 메서드가 덮어쓴다.

```jsx
Array.prototype.sum = function(){
	return this.reduce((acc, cur) => acc + cur, 0);
}

[1, 2].sum(); // 3
```

그러므로 표준 빌트인 객체를 확장할 때는 심벌 값으로 프로퍼티 키를 생성해 안전하게 확장하자.

```jsx
Array.prototype[Symbol.for('sum')] = function(){
	return this.reduce((acc, cur) => acc + cur, 0);
}

[1, 2][Symbol.for('sum')](); // 3
```

## 33.7 Well-known Symbol

자바스크립트가 기본으로 제공하는 빌트인 심벌 값인 Well-known Symbol을 알아보자. 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.

<img width="632" alt="스크린샷 2023-11-27 오후 11 52 48" src="https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/2b849ef3-3a1d-4b75-bb41-28aa388ac077">

Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.

예를 들어 Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection과 같이 for…of문으로 순회 가능한 빌트인 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가진다. Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다. 빌트인 이터러블은 이터레이션 프로토콜을 준수한다.

만약 일반 객체를 이터러블처럼 동작하도록 구현하고 싶으면 이터레이션 프로토콜을 따르면 된다. Symbol.itertator를 키로 갖는 메서드를 객체에 추가하고, 이터레이터를 반환하도록 구현하면 된다.

```jsx
const iterable = {
	[Symbol.iterator](){
		let cur = 1;
		const max = 5;
		return {
			next(){
				return { value: cur++, done: cur > max + 1 };
			}
		}
	}
}

for (const num of iterable){
	console.log(num); // 1 2 3 4 5
}
```

Symbol.iterator는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않는다.

심벌을 활용하면 중복되지 않는 상수 값을 생성하고 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해(하위 호환성 보장) 도입되었다.
