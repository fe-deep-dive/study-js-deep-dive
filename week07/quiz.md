## Quiz
- 진행자 : 박진아
- 날짜 : Sep 20 2023  
---
<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 디음 중 틀린 것은? 그리고 이유는?
![IMG_38170D79C219-1](https://github.com/dev-hamster/study-js-deep-dive/assets/123740296/fd4ff381-716d-4255-93c1-91e2cccc96c5)
```
1. 객체는 __proto__를 통해 직접적으로 [[Prototype]]을 접근할 수 있다.
2. 프로토타입은 constructor를 통해 생성자 함수를 접근할 수 있다.
3. 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.
4. 생성자 함수.prototype === 객체.__proto__ 는 true 이다. 
```
<!--
정답: 1. __proto__는 간접적으로 [[Prototype]]을 접근한다. 참고로 접근자 프로퍼티이므로 get, set 속성을 갖고있어 객체의 [[Prototype]]을 변경할 수 있다. 
-->

> 2. 모든 객체는 Object.prototype을 갖고있다?
<!--
정답: O. Object.prototype은프로토타입 체인의 종점인 최상위 객체이므로 모든 객체가 상속받는다.
-->

> 3. 어떤게 출력될까? 그리고 이유는?
```jsx
function Panda(age, name){
    this.age = age;
    this.name = name;
}

Panda.prototype.eat = function(){
    if(this.age >= 1) console.log("나는 죽순을 먹어용");
    else console.log("나는 우유를 먹어용");
}

const fubao = new Panda(4, '푸바오');

fubao.eat(); // ?
```

<!-- 
정답: 나는 죽순을 먹어용
프로토타입 체인이 일어난다. 
-->

> 4. 3번의 과정을 설멍해보자
<!--
1. fubao 객체에는 eat이라는 메소드가 없어 프로토타입 체인을 따라 [[Prototype]]에 바인딩 된 프로토타입 Panda.prototype으로 이동한다. 
그리고 Panda.prototype의 eat 메소드를 검색하고 호출을 한다. 
-->

> 5. [[Prototype]] vs prototype
<!--
prototype: 객체가 상속받는 프로토타입 객체
Prototype: 프로토타입 객체를 가져오는 비표준 접근법
-->
