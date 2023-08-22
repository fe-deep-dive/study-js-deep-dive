## Quiz
- 진행자 : 박지영
- 날짜 : Aug 22 2023
---
<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 객체가 null 또는 undefined가 아닌지 확인하기 위한 연산자로 논리 연산자 &&와 옵셔널 체이닝 연산자 ?.가 있다. 두 연산자의 차이점과 아래 코드의 출력 결과를 말하라.

```javascript
var str = '';

var length = str && str.length;
console.log(length);

var length2 = str?.length;
console.log(length2);
```

<!--
답: 논리 연산자 &&는 좌항 피연산자가 Falsy값(false, undefined, null, 0, -0, NaN, '')이면 좌항 피연산자를 그대로 반환한다. 하지만 옵셔널 체이닝 연산자 ?.는 좌항 피연산자가 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

console.log(length)  // ''
console.log(length2) // 0
-->
</br>

> 2. 다음 코드의 프로퍼티 키의 타입과 해당 타입인 이유를 말하라.

```javascript
var foo = {
  0: 1,
  'hello': 'world',
  '': null,
};
```

<!--
답: 모두 문자열이다.
프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로 문자열로 변환된다.
-->
</br>

> 3. 다음 코드의 출력 결과를 말하고 이유를 설명하라.

```javascript
var foo = {
  name: 'Jay',
  age: 20,
  'last-name': 'Lee',
};

console.log(foo.name);        // 1번
console.log(foo.hello);       // 2번
console.log(foo.[age]);       // 3번
console.log(foo.'last-name'); // 4번
```

<!--
답: 1. 'Jay' 2. undefined 3. ReferenceError: age is not defined 4. SyntaxError: Unexpected String
1. 올바른 접근법이다.
2. 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.
3. 대괄호 프로퍼티 접근 연산자 내에서 따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석한다. 선언된 age가 없기 때문에 ReferenceError가 발생한다.
4. 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름이면 반드시 대괄호 표기법을 사용해야 한다.
-->
</br>

> 4. 다음 코드의 출력 결과를 말하고 이유를 설명하라.

```javascript
const object1 = { x: { y: 1 } };
const object2 = { x: { y: 1 } };
const object3 = { ...object1 };

console.log(object1 === object2);         // 1번
console.log(object1 === object3);         // 2번
console.log(object1.x === object2.x);     // 3번
console.log(object1.x.y === object2.x.y); // 4번
console.log(object1.x.y === object3.x.y); // 5번
```

<!--
답: 1. false 2. false 3. false 4. true 5. true
1. 객체 리터럴은 평가될 때마다 객체를 생성한다. object1과 object2는 다른 메모리에 저장된 별개의 객체이다. 따라서 false이다.
2. 얕은 복사로 생성된 객체는 원본과는 다른 객체이다. 따라서 false이다.
3. 객체 리터럴은 평가될 때마다 객체를 생성한다. object1.x와 object2.x는 다른 메모리에 저장된 별개의 객체이다. 따라서 false이다.
4. 프로퍼티 값을 참조하는 object1.x.y와 object2.x.y는 값으로 평가될 수 있는 표현식이다. 두 표현식 모두 원시 값 1로 평가된다. 따라서 true이다.
5. 얕은 복사는 객체에 중첩되어 있는 객체의 경우 참조 값을 복사한다. 따라서 두 표현식 모두 원시 값 1로 평가된다. 따라서 true이다.
-->
</br>
