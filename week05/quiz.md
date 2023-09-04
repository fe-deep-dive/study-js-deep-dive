## Quiz
- 진행자 : 박지영
- 날짜 : Sep 5 2023
---
<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. var 키워드로 선언한 변수와 let 키워드로 선언한 변수의 차이점을 말하라.

<!--
답: 
var 키워드로 선언한 변수는 중복 선언이 가능하고 함수 레벨 스코프를 가진다.
let 키워드로 선언한 변수는 중복 선언이 금지되고 블록 레벨 스코프를 가진다.
-->
</br>

> 2. 다음 코드의 출력 결과와 이유를 말하라.

```javascript
console.log(fu);  // 1.

fu = 123;

console.log(fu);  // 2.
console.log(bao); // 3.

var fu;
let bao = 1;

{
  console.log(bao);// 4.
  let bao = 3;
}
```

<!--
답: 1. undefined 2. 123 3. ReferenceError: bao is not defined 4. ReferenceError: Cannot access 'bao' before initialization
1. var 키워드로 선언한 변수는 변수 호이스팅에 의해 변수 선언문 이전에 이미 선언되어 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 undefined를 반환한다.
2. 변수 선언문 이전에 선언된 변수에 값을 할당했기 때문에 할당된 값이 출력된다.
3. let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행되기 때문에 변수 선언문 이전에 초기화 단계는 실행되지 않는다. 초기화되지 않은 변수에 접근하려고 하면 참조 에러가 발생한다.
4. let 키워드로 선언한 변수도 호이스팅이 발생하기 때문에 전역 변수 bao를 참조하지 않고 참조 에러가 발생한다.
-->
</br>

> 3. 2번 문제의 3번과 같은 결과가 발생하는 구간의 이름을 말하라.

<!--
답: 일시적 사각지대(Temporal Dead Zone: TDZ)
스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대라고 부른다.
-->
</br>

> 4. 1번 메서드의 역할과 2~5번의 실행 결과를 말하라.

```javascript
const person = { name: 'Lee' };

Object.seal(person);    // 1.

person.age = 20;
console.log(person);    // 2.

delete person.name;
console.log(person);    // 3.

Object.defineProperty(person, 'name', { configurable: true });  // 4.

Object.defineProperty(person, 'name', { writable: false });     // 5.
```

<!--
답: 1. 객체 밀봉 2. {name: "Lee"} 3. {name: "Lee"} 4. TypeError: Cannot redefine property: name 5. {name: "Lee"}
1. 객체 밀봉은 프로퍼티 추가 및 삭제, 프로퍼티 어트리뷰트 재정의를 금지한다. 즉, 밀봉된 객체는 읽기와 쓰기만 가능하다.
2, 3. 프로퍼티 추가 및 삭제가 금지되므로 무시된다.
4. 프로퍼티 어트리뷰트 재정의가 금지되므로 TypeError가 발생한다.
5. 프로퍼티 어트리뷰트 [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.
-->
</br>
