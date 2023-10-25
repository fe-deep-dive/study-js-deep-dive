## Quiz

- 진행자 : 서범석
- 날짜 : Oct 24 2023 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 생성자 함수와 클래스 방식으로 객체를 생성했을 때, console에 찍히는 값과 이유를 말해주세요.

```jsx
// 생성자 함수
const JS = (function () {
  function JS() {
    // 여긴 생성자 함수 어쩌고 저쩌고
  }
  console.log(this);

  return JS;
})();

// 클래스
class TS {
  typeisGood = () => {
    console.log(this);
  };

  static isGood() {
    console.log(this);
  }
}
```

<!--
답: 생성자 함수는 window, 클래스의 화살표 함수는 TS 인스턴스, 정적 메서드는 TS 클래스.
설명: 즉시실행함수의 this = 함수 안에서의 this, 즉 전역 객체 리턴
     클래스의 화살표 함수의 this = 상위 스코프 = TS 인스턴스
     정적 메서드의 this = 클래스
-->

<br>

> 2. 이 코드는 어떻게 동작하는지 설명해주세요.

```jsx
class A {
  constructor() {
    console.log(new B());
  }
}

class B {}

new A();
```

<!--
답: B {}, new A()를 실행한 위치는 이미 B가 선언되었다.
-->
