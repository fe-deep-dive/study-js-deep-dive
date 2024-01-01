## 39장 DOM

---

38.3절 "HTML 파싱과 DOM 생성"에서 살펴본 바와 같이 브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM을 생성한다.

DOM<sup>Document Object Model</sup>은 HTML 문서의 **계층적 구조와 정보**를 표현하며 이를 제어할 수 있는 **API, 즉 프로퍼티와 메서드**를 제공하는 **트리 자료구조**이다.

### 39.1 노드

**39.1.1 HTML 요소와 노드 객체**

HTML 요소<sup>HTML element</sup>는 HTML 문서를 구성하는 개별적인 요소를 의미한다.

<p align="center"><img width="60%" src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/c143f2c3-71f6-4d9c-b16c-ea0c9e527e7c"></p>

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 **요소 노드 객체**로 변환된다.

이때 HTML 요소의 어트리뷰트는 **어트리뷰트 노드**로, 텍스트 콘텐츠는 **텍스트 노드**로 변환된다.

<p align="center"><img width="60%" src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/0a2521f1-0bbc-4441-9079-fceaec133a85"></p>

HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 갖는다. 즉, HTML 요소의 콘텐츠 영역에는 텍스트 뿐만 아니라 다른 HTML 요소도 포함할 수 있다.

이때 HTML 요소 간에는 중첩 관계에 의해 **계층적인 부자**<sup>parent-child</sup> 관계가 형성된다. 이러한 HTML 요소 간의 부자 관계를 반영하여 모든 노드 객체들을 트리 자료 구조로 구성한다.

- 트리 자료구조

  트리 자료구조<sup>tree data structure</sup>는 부모 노드<sup>parent node</sup>와 자식 노드<sup>child node</sup>로 구성되어 노드 간의 계층적 구조(부자, 형제 관계)를 표현하는 비선형 자료구조를 말한다.

  트리 자료구조는 하나의 최상위 노드에서 시작한다. 최상위 노드는 부모 노드가 없으며, 루트 노드<sup>root node</sup>라 한다. 루트 노드는 0개 이상의 자식 노드를 갖는다. 자식 노드가 없는 노드를 리프 노트<sup>leaf node</sup>라 한다.

  <p align="center"><img width="40%" src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/530a8fd8-ae23-4d95-9b94-d4389dec3a8b"></p>

**노드 객체들로 구성된 트리 자료구조를 DOM<sup>Document Object Model</sup>이라 한다.**

노드 객체의 트리로 구조화되어 있기 때문에 DOM을 **DOM 트리**라고 부르기도 한다.

**39.1.2 노드 객체의 타입**

다음 HTML 문서를 렌더링 엔진이 파싱한다고 생각해보자.

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<lilnk rel="stylesheet" href="style.css">
</head>
<body>
	<ul>
		<li id="apple">Apple</li>
		<li id="banana">Banana</li>
		<li id="orange">Orange</li>
	</ul>
	<script src="app.js"></script>
</body>
</html>
```

렌더링 엔진은 위 HTML 문서를 파싱하여 다음과 같이 DOM을 생성한다.

<p align="center"><img width="80%" src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/a06bef44-2135-4c05-b068-8164ee403270"></p>

노드 객체는 총 12개의 종류가 있는데, 이 중 중요한 노드 타입은 다음과 같이 4가지다.

- 문서 노드<sup>document node</sup>

  문서 노드는 DOM 트리의 최상위에 존재하는 **루트 노드**로서 document 객체를 가리킨다.

  document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.

  브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. 즉, **HTML 문서당 document 객체는 유일하다.**
  문서 노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 **진입점**<sup>entry point</sup> 역할을 담당한다. 즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.
- 요소 노드<sup>element node</sup>

  요소 노드는 HTML 요소를 가리키는 객체다. 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 따라서 요소 노드는 **문서의 구조를 표현**한다고 할 수 있다.
- 어트리뷰트 노드<sup>attribute node</sup>

  어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다. 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 어트리뷰트 노드는 부모 노드와 연결되어 있지 않으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하려면 먼저 요소 노드에 접근해야 한다.
- 텍스트 노드<sup>text node</sup>

  텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다. 텍스트 노드는 **문서의 정보를 표현**한다고 할 수 있다. 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노트이다. 즉, 텍스트 노드는 **DOM 트리의 최종단**이다.

위 4가지 노드 타입 외에도 Comment 노드, DocumentType 노드, DocumentFragment 노드 등 총 12개의 노드 타입이 있다.

**39.1.3 노드 객체의 상속 구조**

DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조이다. 노드 객체는 DOM API를 사용하여 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.

DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체<sup>host objects</sup>다. 하지만 노드 객체도 자바스크립트 객체이므로 **프로토타입에 의한 상속 구조**를 갖는다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/b084000b-63fd-440b-9860-fd9a12f21c66"></p>

모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다. 추가적으로 문서 노드는 Document, HTMLDocument 인터페이스를 상속받고 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 각각 상속받는다.

요소 노드는 Element 인터페이스를 상속받는다. 또한 요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 인터페이스를 상속받는다.

input 요소를 파싱하여 객체화한 input 요소 노드 객체를 예로 들어 프로토타입 체인 관점에서 살펴보자.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/1463e90c-5d99-4e3b-a78b-c153e8fd0abb"></p>

input 요소 노드 객체는 HTMLInputElement, HTMLElement, Element, Node, EventTarget, Object의 prototype에 바인딩되어 있는 프로토타입 객체를 상속받는다. 즉, input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="text" />
    <script>
      // input 요소 노드 객체를 선택
      const $input = document.querySelector('input');

      // input 요소 노드 객체의 프로토타입 체인
      console.log(
        Object.getPrototypeOf($input) === HTMLInputElement.prototype,
        Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
        Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
        Object.getPrototypeOf(Element.prototype) === Node.prototype,
        Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
        Object.getPrototypeOf(EventTarget.prototype) === Object.prototype,
      ); // 모두 true
    </script>
  </body>
</html>
```

input 요소 노드 객체는 다음과 같이 특성을 갖는 객체이며, 이러한 기능들을 상속을 통해 제공받는다.

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인할 수 있다. (???)

모든 노드 객체는 EveneTarget 인터페이스를 상속받기 때문에 공통적으로 이벤트를 발생시킬 수 있다. 또한 Node 인터페이스를 상속받기 때문에 트리 탐색 기능(Node.parentNode, node.childNodes 등)이나 노드 정보 제공 기능(Node.nodeType, Node.nodeName 등)을 갖는다.

input 요소 노드 객체와 div 요소 노드 객체는 공통적으로 HTMLElement 인터페이스를 상속받기 때문에 HTML 요소의 스타일을 나타내는 style 프로퍼티가 있다.

하지만 HTML 요소의 종류에 따라 각각 필요한 기능을 제공하는 인터페이스(HTMLInputElement, HTMLDivElement 등)가 다르기 때문에 input 요소 노드 객체는 value 프로퍼티가 있지만 div 요소 노드 객체는 없다.

> 💡
> 지금까지 살펴본 바와 같이 DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API<sup>Application Programming Interface</sup>로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

**프론트엔드 개발자에게 중요한 것은 HTML로 단순히 뷰를 구성하는 것을 넘어 DOM API를 사용하여 노드에 접근하고 HTML의 구조나 내용 또는 스타일 등을 동적으로 변경하는 방법을 익히는 것이다.**

### 39.2 요소 노드 취득

**39.2.1 id를 이용한 요소 노드 취득**

Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다. getElementById 메서드는 Document.prototype의 프로퍼티다. 따라서 반드시 문서 노드인 document를 통해 호출해야 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'banana'인 요소 노드를 탐색하여 반환한다.
      const $elem = document.getElementById('banana');
      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```

id 값은 HTML 문서 내에서 유일한 값이어야 하지만 여러 개 존재하더라도 어떠한 에러도 발생하지 않는다. 이러한 경우 getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환한다.

만약 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 getElementById 메서드는 null을 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="banana">Apple</li>
      <li id="banana">Banana</li>
      <li id="banana">Orange</li>
    </ul>
    <script>
      // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      let $elem = document.getElementById('banana');
      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';

      // id 값이 'grape'인 요소 노드를 탐색하여 반환한다. null이 반환된다.
      $elem = document.getElementById('grape');
      $elem.style.color = 'red'; // -> TypeError: Cannot read properties of null
    </script>
  </body>
</html>
```

HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      // id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
      console.log(foo === document.getElementById('foo')); // true

      // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
      delete foo;
      console.log(foo); // <div id="foo"></div>
    </script>
  </body>
</html>
```

단, id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      let foo = 1;

      // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
      console.log(foo); // 1
    </script>
  </body>
</html>
```


**39.2.2 태그 이름을 이용한 요소 노드 취득**

Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다.getElementsByTagName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName('li');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach((elem) => {
        elem.style.color = 'red';
      });
    </script>
  </body>
</html>
```

HTML 문서의 모든 요소 노드를 취득하려면 getElementsByTagName 메서드의 인수로 '\*'을 전달한다.

```jsx
const $all = document.getElementsByTagName('*');
// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script, apple: li#apple, banana: li#banana, orange: li#orange]
```

getElementsByTagName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.

Document.prototype.getElementsByTagName 메서드는 DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.

하지만 Element.prototype.getElementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $lisFromDocument = document.getElementsByTagName('li');
      console.log($lisFromDocument);
      // -> HTMLCollection(4) [li#apple, li#banana, li#orange, li, apple: li#apple, banana: li#banana, orange: li#orange]

      // ul#fruists 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $lisFromFruits = $fruits.getElementsByTagName('li');
      console.log($lisFromFruits);
      // -> HTMLCollection(3) [li#apple, li#banana, li#orange, apple: li#apple, banana: li#banana, orange: li#orange]
    </script>
  </body>
</html>
```

만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByTagName 메서드는 빈 HTMLCollection 객체를 반환한다.

**39.2.3 class를 이용한 요소 노드 취득**

Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다. getElementsByTagName 메서드와 마찬가지로 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값이 'fruit'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('fruit');

      [...$elems].forEach((elem) => {
        elem.style.color = 'red';
      });

      // class 값이 'fruit apple'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $apples = document.getElementsByClassName('fruit apple');
      [...$apples].forEach((elem) => {
        elem.style.color = 'blue';
      });
    </script>
  </body>
</html>
```

getElementsByTagName 메서드와 마찬가지로 getElementsByClassName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.

Document.prototype.getElementsByClassName 메서드는 DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.

하지만 Element.prototype.getElementsByClassName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $bananasFromDocument = document.getElementsByClassName('banana');
      console.log($bananasFromDocument);
      // -> HTMLCollection(2) [li.banana, div.banana]

      // #fruits 요소의 자손 노드 중에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $bananasFromFruits = $fruits.getElementsByClassName('banana');
      console.log($bananasFromFruits);
      // -> HTMLCollection [li.banana]
    </script>
  </body>
</html>
```

만약 인수로 전달된 class 이름을 갖는 요소가 존재하지 않는 경우 getElementsByClassName 메서드는 빈 HTMLCollection 객체를 반환한다.

**39.2.4 CSS 선택자를 이용한 요소 노드 취득**

CSS 선택자<sup>selector</sup>는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

```css
/* 전체 선택자: 모든 요소를 선택 */
* { ... }
/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p { ... }
/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo { ... }
/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo { ... }
/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type='text'] { ... }
/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }
/* 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }
/* 인접 형제 선택자: p 요소의 형제 요소 중 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }
/* 일반 형제 선택자: p 요소의 형제 요소 중 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }
/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover { ... }
/* 가상 요소 선택자: p 요소의 콘텐츠 앞에 위치하는 공간을 선택
	 일반적으로 content 프로퍼티와 함께 사용된다. */
p::before { ... }
```

<details>
<summary>가상 클래스 vs 가상 요소</summary>
가상 클래스란 실제로 존재하는 요소에 가상으로 클래스를 부여해서 스타일을 제어하는 것이고, 가상 요소란 실제로 존재하지 않는 가상의 요소를 만들어 스타일을 주는 것이다.
</details>


Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.

- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다.
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환한다.
- 인수로 전달한 CSS 선택자를 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다. querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 유사 배열 객체이면서 이터러블이다.

- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환한다.
- 인수로 전달한 CSS 선택자를 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환한다.
      const $elem = document.querySelector('.banana');
      $elem.style.color = 'red';

      // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환한다.
      const $elems = document.querySelectorAll('ul > li');
      console.log($elems);
      // -> NodeList(3) [li.apple, li.banana, li.orange]

      $elems.forEach((e) => {
        e.style.color = 'blue';
      });
    </script>
  </body>
</html>
```

HTML 문서 내의 모든 요소를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 '\*'을 전달한다.

```jsx
const $all = document.querySelectorAll('*');
console.log($all);
// -> NodeList(8) [html, head, body, ul, li.apple, li.banana, li.orange, script]
```

getElementsByTagName, getElementsByClassName 메서드와 마찬가지로 querySelector, querySelectorAll 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.

Document.prototype.getElementsByClassName 메서드는 DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.

하지만 Element.prototype.getElementsByClassName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

CSS 선택자 문법을 사용하는 querySelector, querySelectorAll 메서드는 getElementById, getElementBy\*\*\* 메서드보다 다소 느린 것으로 알려져 있다. 하지만 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다.

> 💡
> id 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElementById 메서드를 사용하고 그 외의 경우에는 querySelector, querySelectorAll 메서드를 사용하는 것을 권장한다.


**39.2.5 특정 요소 노드를 취득할 수 있는지 확인**

Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      const $apple = document.querySelector('.apple');

      // $apple 노드는 '#fruits > li.apple'로 취득할 수 있다.
      console.log($apple.matches('#fruits > li.apple')); // true

      // $apple 노드는 '#fruits > li.banana'로 취득할 수 없다.
      console.log($apple.matches('#fruits > li.banana')); // false
    </script>
  </body>
</html>
```

Element.prototype.matches 메서드는 이벤트 위임을 사용할 때 유용하다. 이에 대해서는 40.7절 "위벤트 위임"에서 자세히 살펴보자~!


**39.2.6 HTMLCollection과 NodeList**

DOM 컬렉션 객체인 HTMLCollection과 NodeList는 **DOM API가 여러 개의 결과값을 반환**하기 위한 DOM 컬렉션 객체다.

HTMLCollection과 NodeList는 모두 유사 배열 객체이면서 이터러블이다. 따라서 for …of 문으로 순회할 수 있으며 스프레드 문법을 사용할 수 있다.

HTMLCollection과 NodeList의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 **살아 있는**<sup>live</sup> **객체**라는 것이다. 단, HTMLCollection은 언제나 live 객체로 동작하고 NodeList는 대부분의 경우 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있다.

- HTMLCollection

  getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는<sup>live</sup> DOM 컬렉션 객체다.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="red">Apple</li>
        <li class="red">Banana</li>
        <li class="red">Orange</li>
      </ul>
      <script>
        // class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
        const $elems = document.getElementsByClassName('red');
        // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
        console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

        // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
        for (let i = 0; i < $elems.length; i++) {
          $elems[i].className = 'blue';
        }

        // HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
        console.log($elems); // HTMLCollection [li.red]
      </script>
    </body>
  </html>
  ```

  1. 첫 번째 반복(i === 0)

     $elems[0]은 첫 번째 li 요소다. 이 요소의 class 값이 'red'에서 'blue'로 변경된다. 이때 getElementByClassName 메서드의 인자로 전달한 'red'와 더는 일치하지 않기 때문에 $elems에서 실시간으로 제거된다.

  2. 두 번째 반복(i === 1)

     첫 번째 반복에서 첫 번째 li 요소는 $elems에서 제거되었으므로 $elems[1]은 세 번째 li 요소다. 이 세 번째 요소의 class 값도 'blue'로 변경되고 마찬가지로 HTMLCollection 객체인 $elems에서 실시간으로 제외된다.

  3. 세 번째 반복 (i === 2)

     $elems.length는 1이므로 for문의 조건식 i < $elems.length가 false로 평가되어 종료된다.

     이처럼 HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경할 때 주의해야 한다.
     
     이 문제는 for문을 역방향으로 순회하는 방법으로 회피할 수 있다.

  ```jsx
  for (let i = $elems.length - 1; i >= 0; i--) {
    $elems[i].className = 'blue';
  }
  ```

  또는 while문을 사용하여 HTMLCollection 객체에 노드 객체가 남아있지 않을 때까지 무한 반복하는 방법으로 회피할 수도 있다.

  ```jsx
  let i = 0;
  while ($elems.length > i) {
    $elems[i].className = 'blue';
  }
  ```

  더 간단한 해결책은 HTMLCollection 객체를 사용하지 않는 것이다. **유용한 배열의 고차 함수(forEach, map, filter, reduce 등)을 사용하자.**

  ```jsx
  [...$elems].forEach((elem) => (elem.className = 'blue'));
  ```

- NodeList

  HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메서드를 사용하는 방법도 있다. querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환하는데, 이때 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는<sup>non-live</sup> 객체다.

  ```jsx
  // querySelectorAll은 DOM 컬렉션 객체인 NodeList를 반환한다.
  const $elems = document.querySelectorAll('.red');

  // NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
  $elems.forEach((elem) => (elem.className = 'blue'));
  ```

  NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있고 이 외에도 item, entries, keys, values 메서드를 제공한다.

  NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않는 non-live 객체로 동작한다. 하지만 **childNodes 프로퍼티가 반환하는 NodeList 객체**는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 **live 객체로 동작하므로 주의가 필요하다.**

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
      <script>
        const $fruits = document.getElementById('fruits');

        // childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
        const { childNodes } = $fruits;
        console.log(childNodes instanceof NodeList); // true

        console.log(childNodes); // NodeList(5) [text, li, text, li, text]

        for (let i = 0; i < childNodes.length; i++) {
          // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes가 실시간으로 반영된다.
          // 따라서 첫 번째, 세 번째, 다섯 번째 요소만 삭제된다.
          $fruits.removeChild(childNodes[i]);
        }

        console.log(childNodes); // NodeList(2) [li, li]
      </script>
    </body>
  </html>
  ```

  이처럼 HTMLCollection이나 NodeList 객체는 예상과 다르게 동작할 때가 있어 다루기 까다롭다.

    > 💡
    > 따라서 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.
    
    HTMLCollection과 NodeList 객체는 모두 유사 배열 객체이면서 이터러블이기 때문에 스프레드 문법이나 Array.from 메서드를 사용하여 간단히 배열로 변환할 수 있다.
    
    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li>Apple</li>
          <li>Banana</li>
        </ul>
        <script>
          const $fruits = document.getElementById('fruits');
    
          const { childNodes } = $fruits;
    
          // 스프레드 문법을 사용하여 NodeList 객체를 배열로 변환한다.
          [...childNodes].forEach((childNode) => {
            $fruits.removeChild(childNode);
          });
    
          console.log(childNodes); // NodeList []
        </script>
      </body>
    </html>
    ```

### 39.3 노드 탐색

요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색<sup>traversing, node walking</sup>할 수 있다.

Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/6d6fb12d-f055-48b1-aac3-a461e2a560be"></p>

parentNode, previousSibling, firstChild, childNodes 프로퍼티는 Node.prototype이 제공하고,

프로퍼티 키에 Element가 포함된 previousElementSibling, nextElementSibling과 children 프로퍼티는 Element.prototype이 제공한다.

노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 **읽기 전용 접근자 프로퍼티**다. 읽기 전용 접근자 프로퍼티에 값을 할당하면 아무런 에러 없이 무시된다.

**39.3.1 공백 텍스트 노드**

HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백<sup>white space</sup> 문자는 텍스트 노드를 생성한다. 이를 공백 테스트 노드라 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

위 HTML 문서는 다음과 같은 DOM을 생성한다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/9a218d8b-f27d-43b5-82df-a6b79311164d"></p>

따라서 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다. 다음과 같이 인위적으로 HTML 문서의 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않는다. 하지만 가독성이 좋지 않으므로 권장하지 않는다.

```html
<ul id="fruits"><li
  id="apple">Apple</li><li
  id="banana">Banana</li><li
  id="orange">Orange</li></ul>
```

**39.3.2 자식 노드 탐색**

자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

| 프로퍼티 | 설명 |
| --- | --- |
| Node.prototype.childNodes | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환한다.<br>childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다. |
| Element.prototype.children | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다.<br>children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다.                        |
| Node.prototype.firstChild | 첫 번째 자식 노드를 반환한다.<br>firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다. |
| Node.prototype.lastChild | 마지막 자식 노드를 반환한다.<br>lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.   |
| Element.prototype.firstElementChild | 첫 번째 자식 요소 노드를 반환한다.<br>firstElementChild 프로퍼티는 요소 노드만 반환한다.            |
| Element.prototype.lastElementChild | 마지막 자식 요소 노드를 반환한다.<br>lastElementChild 프로퍼티는 요소 노드만 반환한다. |

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // #fruits 요소의 모든 자식 노드를 탐색한다.
    // childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있다.
    console.log($fruits.childNodes);
    // NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

    // #fruits 요소의 모든 자식 노드를 탐색한다.
    // children 프로퍼티가 반환한 HTMLCollection에는 요소 노드만 포함되어 있다.
    console.log($fruits.children);
    // HTMLCollection(3) [li.apple, li.banana, li.orange]

    // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
    console.log($fruits.firstChild); // #text

    // #fruits 요소의 마지막 자식 노드를 탐색한다.
    // lastChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
    console.log($fruits.lastChild); // #text

    // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstElementChild 프로퍼티는 요소 노드만 반환한다.
    console.log($fruits.firstElementChild); // <li class="apple">Apple</li>

    // #fruits 요소의 마지막 자식 노드를 탐색한다.
    // lastElementChild 프로퍼티는 요소 노드만 반환한다.
    console.log($fruits.lastElementChild); // <li class="orange">Orange</li>
  </script>
</html>
```

**39.3.3 자식 노드 존재 확인**

자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드를 사용한다.

자식 노드가 존재하면 true, 존재하지 않으면 false를 반환한다. 단, hasChildNodes 메서드는 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');
    console.log($fruits.hasChildNodes()); // true
  </script>
</html>
```

자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 children.length 또는 Element 인터페이스의 childElementCount 프로퍼티를 사용한다.

```jsx
console.log(!!$fruits.children.length); // false
console.log(!!$fruits.childElementCount); // false
```

**39.3.4 요소 노드의 텍스트 노드 탐색**

요소 노드의 텍스트 노드는 요소 노드의 자식 노드다. 따라서 firstChild 프로퍼티로 접근할 수 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    console.log(document.getElementById('foo').firstChild); // "Hello"
  </script>
</html>
```

**39.3.5 부모 노드 탐색**

부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다.

텍스트 노드는 DOM 트리의 최종단 노드인 리프 노트<sup>left node</sup>이므로 부모 노드가 텍스트 노드인 경우는 없다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
    const $banana = document.querySelector('.banana');

    // .banana 요소의 부모 노드를 탐색한다.
    console.log($banana.parentNode); // <ul id="fruits">...</ul>
  </script>
</html>
```

**39.3.6 형제 노드 탐색**

부모 노드가 같은 형제 노드를 탐색하려면 다음과 같은 노드 탐색 프로퍼티를 사용한다.

단, 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않는다.

| 프로퍼티 | 설명 |
| --- | --- |
| Node.prototype.previousSibling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환한다.<br>previousSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| Node.prototype.nextSibling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다.<br>nextSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| Element.prototype.previousElementSibling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환한다.<br>previousElementSibling 프로퍼티는 요소 노드만 반환한다. |
| Element.prototype.nextElementSibling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다.<br>nextElementSibling 프로퍼티는 요소 노드만 반환한다. |

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // $fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstChild 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { firstChild } = $fruits;
    console.log(firstChild); // #text

    // $fruits 요소의 첫 번째 자식 노드(텍스트 노드)의 다음 형제 노드를 탐색한다.
    // nextSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { nextSibling } = firstChild;
    console.log(nextSibling); // li.apple

    // li.apple 요소의 이전 형제 노드를 탐색한다.
    // previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { previousSibling } = nextSibling;
    console.log(previousSibling); // #text

    // #fruits 요소의 첫 번째 자식 요소 노드를 탐색한다.
    // firstElementChild 프로퍼티는 요소 노드만 반환한다.
    const { firstElementChild } = $fruits;
    console.log(firstElementChild); // li.apple

    // $fruits 요소의 첫 번째 자식 요소 노드(li.apple)의 다음 형제 노드를 탐색한다.
    // nextElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { nextElementSibling } = firstElementChild;
    console.log(nextElementSibling); // li.banana

    // li.banana 요소의 이전 형제 요소 노드를 탐색한다.
    // nextElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { previousElementSibling } = nextElementSibling;
    console.log(previousElementSibling); // li.apple
  </script>
</html>
```

### 39.4 노드 정보 취득

노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용한다.

| 프로퍼티                | 설명                                                       |
| ----------------------- | ---------------------------------------------------------- |
| Node.prototype.nodeType | 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다.<br>- Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환<br>- Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환<br>- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환 |
  | Node.prototype.nodeName | 노드의 이름을 문자열로 반환한다.<br>- 요소 노드: 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환<br>- 텍스트 노드: 문자열 "#text"를 반환<br>- 문서 노드: 문자열 "#document"를 반환 |

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    console.log(document.nodeType); // 9
    console.log(document.nodeName); // #document

    const $foo = document.getElementById('foo');
    console.log($foo.nodeType); // 1
    console.log($foo.nodeName); // DIV

    const $textNode = $foo.firstChild;
    console.log($textNode.nodeType); // 3
    console.log($textNode.nodeName); // #text
  </script>
</html>
```


### 39.5 요소 노드의 텍스트 조작


**39.5.1 nodeValue**

지금부터 살펴 볼 Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 따라서 nodeValue 프로퍼티는 **참조와 할당 모두 가능**하다.

노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트다. 따라서 텍스트 노드가 아닌 노드의 nodeValue 프로퍼티를 참조하면 null을 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 문서 노드의 nodeValue 프로퍼티를 참조한다.
    console.log(document.nodeValue); // null

    // 요소 노드의 nodeValue 프로퍼티를 참조한다.
    const $foo = document.getElementById('foo');
    console.log($foo.nodeValue); // null

    // 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
    const $textNode = $foo.firstChild;
    console.log($textNode.nodeValue); // Hello
  </script>
</html>
```

텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트 노드의 값, 즉 텍스트를 변경할 수 있다.

따라서 요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요하다.

1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색한다. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색한다.
2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 1. #foo 요소 노드의 자식 노드인 텍스트 노드를 취득한다.
    const $textNode = document.getElementById('foo').firstChild;

    // 2. nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.
    $textNode.nodeValue = 'World';

    console.log($textNode.nodeValue); // World
  </script>
</html>
```


**39.5.2 textContent**

Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.

요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환한다. 다시 말해, 요소 노드의 childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값, 즉 텍스트를 모두 반환한다. 이때 HTML 마크업은 무시된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소 노드의 텍스트를 모두 취득한다. 이때 HTML 마크업은 무시된다.
    console.log(document.getElementById('foo').textContent); // Hello world!
  </script>
</html>
```

nodeValue 프로퍼티를 참조하여서도 텍스트를 취득할 수 있다. 다만 textContent 프로퍼티를 사용할 때와 비교해서 코드가 더 복잡하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    console.log(document.getElementById('foo').firstChild.nodeValue); // Hello
    console.log(document.getElementById('foo').lastChild.firstChild.nodeValue); // world!
  </script>
</html>
```

요소 노드의 콘텐츠 영역에 자식 요소 노드가 없고 텍스트만 존재한다면 firstChild.nodeValue와 textContent 프로퍼티는 같은 결과를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <!-- 요소 노드의 콘텐츠 영역에 다른 요소 노드가 없고 텍스트만 존재 -->
    <div id="foo">Hello</div>
  </body>
  <script>
    const $foo = document.getElementById('foo');

    console.log($foo.textContent === $foo.firstChild.nodeValue); // true
  </script>
</html>
```

요소 노드의 textContent 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다.
    // 이때 HTML 마크업이 파싱되지 않는다.
    document.getElementById('foo').textContent = 'Hi <span>there!</span>';
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/f8d86f03-aa15-48c2-84b3-c9434ea5cbba"></p>

참고로 textContent 프로퍼티와 유사한 동작을 하는 innerText 프로퍼티가 있다. innerText 프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋다.

- innerText 프로퍼티는 CSS에 순종적이다. 예를 들어, innerText 프로퍼티는 CSS에 의해 비표시(visibility: hidden;)로 지정된 요소 노드의 텍스트를 반환하지 않는다.
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느리다.


### 39.6 DOM 조작

DOM 조작<sup>DOM manipulation</sup>은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.

DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다. 따라서 DOM 조작은 성능 최적화를 위해 주의해서 다루어야 한다.


**39.6.1 innerHTML**

Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다. 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.
    console.log(document.getElementById('foo').innerHTML);
    // Hello <span>world!</span>
  </script>
</html>
```

textContent 프로퍼티를 참조하면 HTML 마크업을 무시하고 텍스트만 반환하지만 innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환한다.

요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
    document.getElementById('foo').innerHTML = 'Hi <span>there!</span>';
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/72c9167e-2a82-4fb2-8e07-064d1507ba34"></p>

요소 노드의 innerHTML 프로퍼티에 할당한 HTML 마크업 문자열은 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영된다. 이때 사용자로부터 입력받은 데이터<sup>untrusted input data</sup>를 그대로 innerHTML 프로퍼티에 할당하는 것은 **크로스 사이트 스크립팅 공격**<sup>XSS: Cross-Site Scripting Attacks</sup>에 취약하므로 위험하다.

innerHTML 프로퍼티로 스크립트 태그를 삽입하여 자바스크립트가 실행되도록 하는 예제를 살펴보자.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 에러 이벤트를 강제로 발생시켜서 자바스크립트 코드가 실행되도록 한다.
    document.getElementById('foo').innerHTML = '<img src="x" onerror="alert(document.cookie)">';
  </script>
</html>
```

<details>

<summary>HTML 새니티제이션<sup>HTML sanitization</sup></summary>

HTML 새니티제이션은 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능을 말한다. 새니티제이션 함수를 직접 구현할 수도 있겠지만 **DOMPurify 라이브러리**를 사용하는 것을 권장한다.

```jsx
DOMPurify.sanitize('<img src="x" onerror="alert(document.cookie)">');
// => <img src="x">
```

</details>


innerHTML 프로퍼티의 또 다른 단점은 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하면 기존의 모든 자식 노드를 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 DOM을 변경한다는 것이다. 이는 효율적이지 않다.

또한 innerHTML 프로퍼티는 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점도 있다.

> 💡
> innerHTML 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.


**39.6.2 insertAdjacentHTML 메서드**

Element.prototype.insertAdjacentHTML(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

insertAdjacentHTML 메서드는 두 번째 인수로 전달한 HTML 마크업 문자열(DOMString)을 파싱하고 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치(position)에 삽입하여 DOM에 반영한다.

첫 번째 인수로 전달할 수 있는 문자열은 'beforebegin', 'afterbegin', 'beforeend', 'afterend'의 4가지다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/795b49c3-f873-47e4-a52f-85c4bfe7b355"></p>

insertAdjacentHTML 메서드는 기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 **innerHTML 프로퍼티보다 효율적이고 빠르다**. 단, innerHTML 프로퍼티와 마찬가지로 HTML 마크업 문자열을 파싱하므로 **크로스 사이트 스크립팅 공격에 취약**하다는 점은 동일하다.


**39.6.3 노드 생성과 추가**

DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 1. 요소 노드 생성
    const $li = document.createElement('li');

    // 2. 텍스트 노드 생성
    const textNode = document.createTextNode('Banana');

    // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
    $li.appendChild(textNode);

    // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    // 이 과정에서 새롭게 생성한 요소 노드가 DOM에 추가된다.
    // 이때 리플로우와 리페인트가 실행된다.
    $fruits.appendChild($li);
  </script>
</html>
```


**39.6.4 복수의 노드 생성과 추가**

DOM을 변경하는 것은 높은 비용이 드는 처리이므로 가급적 횟수를 줄이는 편이 성능에 유리하다. DOM을 여러 번 변경하는 문제를 회피하기 위해 컨테이너 요소를 사용해 보자.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 컨테이너 요소 노드 생성
    const $container = document.createElement('div');

    ['Apple', 'Banana', 'Orange'].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $container.appendChild($li);
    });

    // 5. 컨테이너 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($container);
  </script>
</html>
```

위 예제는 DOM을 한 번만 변경하므로 성능에 유리하기는 하지만 불필요한 컨테이너 요소(div)가 DOM에 추가되는 부작용이 있다.

이러한 문제는 **DocumentFragment 노드**를 통해 해결할 수 있다. DocumentFragment 노드는 문서, 요소, 어트리뷰트, 텍스트 노드와 같은 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다. DocumentFragment 노드는 위 예제의 컨테이너 요소와 같이 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ['Apple', 'Banana', 'Orange'].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fragment.appendChild($li);
    });

    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
  </script>
</html>
```

이때 실제로 DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행된다.

> 💡
> 따라서 여러 개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.


**39.6.5 노드 삽입**

- 마지막 노드로 추가

  Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.
- 지정한 위치에 노드 삽입

  Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // 요소 노드 생성
      const $li = document.createElement('li');

      // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
      $li.appendChild(document.createTextNode('Orange'));

      // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
      $fruits.insertBefore($li, $fruits.lastElementChild);
    </script>
  </html>
  ```

    <p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/3abb1574-0a12-46fc-a030-e8a1ea5c9d4d"></p>
    
    두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 한다. 그렇지 않으면 DOMException 에러가 발생한다.
    
    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <div>test</div>
        <ul id="fruits">
          <li>Apple</li>
          <li>Banana</li>
        </ul>
      </body>
      <script>
        const $fruits = document.getElementById('fruits');
    
        // 요소 노드 생성
        const $li = document.createElement('li');
    
        // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
        $li.appendChild(document.createTextNode('Orange'));
    
        // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
        $fruits.insertBefore($li, document.querySelector('div')); // DOMException
      </script>
    </html>
    ```
    
    두 번째 인수로 전달받은 노드가 null이면 마지막 자식 노드로 추가된다. 즉, appendChild 메서드처럼 동작한다.


**39.6.6 노드 이동**

DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 즉, 노드가 이동한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 이미 존재하는 요소 노드를 취득
    const [$apple, $banana] = $fruits.children;

    // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
    $fruits.appendChild($apple); // Banana - Orange - Apple

    // 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
    $fruits.insertBefore($banana, $fruits.lastElementChild); // Orange - Banana - Apple
  </script>
</html>
```

**39.6.7 노드 복사**

Node.prototype.cloneNode([deep: true | false]) 메서드는 노드의 사본을 생성하여 반환한다.

매개변수 deep에 true를 인수로 전달하면 노드를 깊은 복사<sup>deep copy</sup>하여 모든 자손 노드가 포함된 사본을 생성하고,

false를 인수로 전달하거나 생략하면 노드를 얕은 복사<sup>shallow copy</sup>하여 노드 자신만의 사본을 생성한다.

얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');
    const $apple = $fruits.firstElementChild;

    // $apple 요소를 얕은 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성된다.
    const $shallowClone = $apple.cloneNode();
    // 사본 요소 노드에 텍스트 추가
    $shallowClone.textContent = 'Banana';
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($shallowClone);

    // #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
    const $deepClone = $fruits.cloneNode(true);
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($deepClone);
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/d825646e-acde-4f9f-9026-79d1ffefd8d7"></p>


**39.6.8 노드 교체**

Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다. 첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달하고, 두 번째 매개변수 oldChild에는 이미 존재하는 교체될 노드를 인수로 전달한다.

oldChild 매개변수에 인수로 전달한 노드는 replaceChild 메서드를 호출한 노드의 자식 노드이어야 한다. 이때 oldChild 노드는 DOM에서 제거된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 기존 노드와 교체할 요소 노드를 생성
    const $newChild = document.createElement('li');
    $newChild.textContent = 'Banana';

    // #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/e6efb43c-7a7c-43ec-9355-4b2cf33fd78e"></p>

**39.6.9 노드 삭제**

Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. 인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // #fruits 요소 노드의 마지막 요소를 DOM에서 삭제
    $fruits.removeChild($fruits.lastElementChild);
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/dc1cabd5-a427-4b91-8ba7-c0078abdbe37"></p>


### 39.7 어트리뷰트


**39.7.1 어트리뷰트 노드와 attributes 프로퍼티**

HTML 문서의 구성 요소인 HTML 요소는 여러 개의 어트리뷰트<sup>attribute</sup>(속성)를 가질 수 있다. **HTML 요소의 동작을 제어하기 위한 추가적인 정보를 제공**하는 HTML 어트리뷰트는 HTML 요소의 시작 태그<sup>start/opening tag</sup>에 **어트리뷰트 이름="어트리뷰트 값"** 형식으로 정의한다.

```html
<input id="user" type="text" value="ungmo2" />
```

HTML 문서가 파싱될 때 HTML 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다. 이때 HTML 어트리뷰트당 하나의 어트리뷰트 노드가 생성된다.

이때 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/ef0fdb1b-6944-4af2-98dd-ed778d7a1ba5"></p>

따라서 요소 노드의 모든 어트리뷰트 노드는 Element.prototype.attributes 프로퍼티로 취득할 수 있다. attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며, NamedNodeMap 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
  </body>
  <script>
    const { attributes } = document.getElementById('user');
    console.log(attributes);
    // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

    console.log(attributes.id.value); // user
    console.log(attributes.type.value); // text
    console.log(attributes.value.value); // ungmo2
  </script>
</html>
```

**39.7.2 HTML 어트리뷰트 조작**

Element.prototype.getAttribute/setAttribute 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어서 편리하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
  </body>
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트 값을 취득
    const inputValue = $input.getAttribute('value');
    console.log(inputValue); // ungmo2

    // value 어트리뷰트 값을 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo
  </script>
</html>
```

특정 HTML 어트리뷰트가 존재하는지 확인하려면 Element.prototype.hasAttribute(attributeName) 메서드를 사용하고, 특정 HTML 어트리뷰트를 삭제하려면 Element.prototype.removeAttribute(attributeName) 메서드를 사용한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
  </body>
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트의 존재 확인
    if ($input.hasAttribute('value')) {
      // value 어트리뷰트 삭제
      $input.removeAttribute('value');
    }

    console.log($input.hasAttribute('value')); // false
  </script>
</html>
```

**39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티**

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(DOM 프로퍼티)가 존재한다. 이 **DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.**

예를 들어, `<input id="user" type="text" value="ungmo2">` 요소가 파싱되어 생성된 요소 노드 객체에는 id, type, value 어트리뷰트에 대응하는 id, type, value 프로퍼티가 존재하며, 이 DOM 프로퍼티들은 HTML 어트리뷰트의 값을 초기값으로 가지고 있다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/8a0b12c4-a708-4582-a78f-264c1a09307c"></p>

DOM 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 따라서 DOM 프로퍼티는 **참조와 변경이 가능**하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
  </body>
  <script>
    const $input = document.getElementById('user');

    // 요소 노드의 value 프로퍼티 값을 변경
    $input.value = 'foo';

    // 요소 노드의 value 프로퍼티 값을 참조
    console.log($input.value); // foo
  </script>
</html>
```

**HTML 어트리뷰트**의 역할은 HTML 요소의 초기 상태를 지정하는 것이다. 즉, HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다.

input 요소의 value 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드의 attributes 프로퍼티에 저장된다. 이와는 별도로 value 어트리뷰트의 값은 요소 노드의 value 프로퍼티에 할당된다.

따라서 input 요소의 요소 노드가 생성되어 첫 렌더링이 끝난 시점까지 어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당된 값은 HTML 어트리뷰트 값과 동일하다.

하지만 첫 렌더링 이후 사용자가 input 요소에 무언가를 입력하기 시작하면 상황이 달라진다.

**요소 노드는 상태<sup>state</sup>를 가지고 있다.** 상태는 사용자의 입력에 의해 변화하는, 살아있는 것이다.

사용자가 input요소의 입력 필드에 "foo"라는 값을 입력하면, input 요소 노드는 **최신 상태**("foo")와 **초기 상태**("ungmo2")를 모두 관리해야 한다. 초기 상태 값을 관리하지 않으면 웹페이지를 처음 표시하거나 새로고침할 때 초기 상태를 표시할 수 없다.

> 💡
> 요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며, 요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.

- 어트리뷰트 노드

  HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다.

  getAttribute 메서드로 취득한 값은 초기 상태 값이다.

  setAttribute 메서드는 초기 상태 값을 변경한다.

    <p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/3e411224-e95c-41c6-94d8-097277d97f14"></p>

- DOM 

  DOM 프로퍼티는 사용자 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.

  단, 모든 DOM 프로퍼티가 사용자의 입력에 의해 변경된 최신 상태를 관리하는 것은 아니다.

  예를 들어, id 어트리뷰트에 대응하는 id 프로퍼티는 사용자 입력과 관계없이 항상 동일한 값으로 연동한다.
- HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

  - id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일한 값으로 연동한다.
  - input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응한다. 하지만 value 어트리뷰는 초기 상태를, value 프로퍼티는 최신 상태를 갖는다.
  - class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
  - for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
  - td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
  - textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
  - 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다(maxlength → maxLength).

- DOM 프로퍼티 값의 

  getAttribute메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.

  하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다. 예를 들어, checkbox 요소의 checked 어트리뷰트 값은 문자열이지만 checked 프로퍼티 값은 불리언 타입이다.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <input type="checkbox" checked />
    </body>
    <script>
      const $checkbox = document.querySelector('input[type=checkbox]');

      // getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
      console.log($checkbox.getAttribute('checked')); // ''

      // DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
      console.log($checkbox.checked); // true
    </script>
  </html>
  ```


**39.7.4 data 어트리뷰트와 dataset 프로퍼티**

data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.

data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul class="users">
      <li id="1" data-user-id="7621" data-role="admin">Lee</li>
      <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
    </ul>
  </body>
  <script>
    const users = [...document.querySelector('.users').children];

    // user-id가 '7621'인 요소 노드를 취득한다.
    const user = users.find((user) => user.dataset.userId === '7621');
    // 요소 노드에서 data-role의 값을 취득한다.
    console.log(user.dataset.role); // admin

    // 요소 노드의 data-role 값을 변경한다.
    user.dataset.role = 'subscriber';
    // dataset 프로퍼티는 DOMStringMap 객체를 반환한다.
    console.log(user.dataset); // DOMStringMap {userId: '7621', role: 'subscriber'}
  </script>
</html>
```

data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.

dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다. DOMStringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있다. 이 프로퍼티로 data 어트리뷰트의 값을 취득하거나 변경할 수 있다.

data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul class="users">
      <li id="1" data-user-id="7621">Lee</li>
      <li id="2" data-user-id="9524">Kim</li>
    </ul>
  </body>
  <script>
    const users = [...document.querySelector('.users').children];

    // user-id가 '7621'인 요소 노드를 취득한다.
    const user = users.find((user) => user.dataset.userId === '7621');

    // 요소 노드에 새로운 data 어트리뷰트를 추가한다.
    user.dataset.role = 'admin';
    console.log(user.dataset); // DOMStringMap {userId: '7621', role: 'admin'}
  </script>
</html>
```


### 39.8 스타일


**39.8.1 인라인 스타일 조작**

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일<sup>inline style</sup>을 취득하거나 추가 또는 변경한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div style="color: red">JS Deep Dive</div>
  </body>
  <script>
    const $div = document.querySelector('div');

    console.log($div.style);
    // CSSStyleDeclaration {0: 'color', accentColor: '', additiveSymbols: '', alignContent: '', alignItems: '', alignSelf: '', …}

    $div.style.color = 'blue';

    $div.style.width = '100px';
    $div.style.height = '100px';
    $div.style.backgroundColor = 'yellow';
  </script>
</html>
```

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/d3410183-28f8-44dd-a22f-7b466ac6c337"></p>

style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다. CSSStyleDeclaration 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며, 이 프로퍼티에 값을 할당하면 해당 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.

CSS 프로퍼티는 케밥 케이스<sup>kebab-case</sup>를 따른다. 이에 대응하는 CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스를 따른다.

```jsx
$div.style.backgroundColor = 'yellow';
```

케밥 케이스의 CSS 프로퍼티를 그대로 사용하려면 대괄호 표기법을 사용한다.

```jsx
$div.style['background-color'] = 'yellow';
```

단위 지정이 필요한 CSS 프로퍼티의 값은 반드시 단위를 지정해야 한다. 단위를 생략하면 해당 CSS 프로퍼티는 적용되지 않는다.

```jsx
$div.style.width = '100px';
```


**39.8.2 클래스 조작**

.으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일을 변경할 수도 있다. 이때 HTML 요소의 어트리뷰트를 조작하려면 class 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티를 사용한다.

단, class 어트리뷰트에 대응하는 DOM 프로퍼티는 className과 classList다.

- className

  Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.

  요소 노드의 className 프로퍼티를 참조하면 class 어트리뷰트 값을 문자열로 반환하고, 요소 노드의 className 프로퍼티에 문자열을 할당하면 class 어트리뷰트 값을 변경한다.

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        .box {
          width: 100px;
          height: 100px;
          background-color: antiquewhite;
        }
        .red {
          color: red;
        }
        .blue {
          color: blue;
        }
      </style>
    </head>
    <body>
      <div class="box red">JS Deep Dive</div>
    </body>
    <script>
      const $box = document.querySelector('.box');

      // .box 요소의 class 어트리뷰트 값을 취득
      console.log($box.className);

      // .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
      $box.className = $box.className.replace('red', 'blue');
    </script>
  </html>
  ```

- classList

  Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        .box {
          width: 100px;
          height: 100px;
          background-color: antiquewhite;
        }
        .red {
          color: red;
        }
        .blue {
          color: blue;
        }
      </style>
    </head>
    <body>
      <div class="box red">JS Deep Dive</div>
    </body>
    <script>
      const $box = document.querySelector('.box');

      // .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
      // classList가 반환하는 DOMTokenList객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) 객체다.
      console.log($box.classList); // DOMTokenList(2) ['box', 'red', value: 'box red']

      $box.classList.replace('red', 'blue');
    </script>
  </html>
  ```

  DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블이다. DOMTokenList 객체는 다음과 같이 유용한 메서드들을 제공한다.

  - add( … className)

    : 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
    ```jsx
    $box.classList.add('fubao'); // -> class="box red fubao"
    $box.classList.add('luibao', 'huibao'); // -> class="box red fubao luibao huibao"
    ```
  - remove( … className)

    : 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다. 일치하는 클래스가 없으면 에러없이 무시된다.
    ```jsx
    $box.classList.remove('fubao'); // -> class="box red luibao huibao"
    $box.classList.remove('luibao', 'huibao'); // -> class="box red"
    $box.classList.remove('aibao'); // -> class="box red"
    ```
  - item(index)

    : 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다. index는 0부터 시작한다.
    ```jsx
    $box.classList.item(0); // -> "box"
    $box.classList.item(1); // -> "red"
    ```
  - contains(className)

    : 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.
    ```jsx
    $box.classList.contains('box'); // -> true
    $box.classList.contains('blue'); // -> false
    ```
  - replace(oldClassName, newClassName)

    : class 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.
    ```jsx
    $box.classList.replace('red', 'blue'); // => class="box blue"
    ```
  - toggle(className[, force])

    : class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
    ```jsx
    $box.classList.toggle('foo'); // -> class="box blue foo"
    $box.classList.toggle('foo'); // -> class="box blue"
    ```
    두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.

    이때 조건식의 평가 결과가 true이면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 추가하고, false이면 제거한다.
    ```jsx
    $box.classList.toggle('foo', true); // -> class="box blue foo"
    $box.classList.toggle('foo', false); // -> class="box blue"
    ```
    이 밖에도 DOMTokenList 객체는 forEach, entries, keys, values, supports 메서드를 제공한다.


**39.8.3 요소에 적용되어 있는 CSS 스타일 참조**

style 프로퍼티는 인라인 스타일만 반환한다. HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 할 경우 getComputedStyle 메서드를 사용한다.

window.getComputedStyle(element[, pseudo]) 메서드는 첫 번째 인수(element)로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환한다.

<details>

<summary>평가된 스타일</summary>

요소 노드에 적용되어 있는 모든 스타일, 즉 링크 스타일, 임베딩 스타일, 인라인 스타일, 자바스크립트에서 적용한 스타일, 상속된 스타일, 기본(user agent) 스타일 등 모든 스타일이 조합되어 최종적으로 적용된 스타일

</details>


```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        color: red;
      }
      .box {
        width: 100px;
        height: 50px;
        background-color: cornsilk;
        border: 1px solid black;
      }
    </style>
  </head>
  <body>
    <div class="box">JS Deep Dive</div>
  </body>
  <script>
    const $box = document.querySelector('.box');

    const computedStyle = window.getComputedStyle($box);
    console.log(computedStyle); // CSSStyleDeclaration 객체

    // 임베딩 스타일
    console.log(computedStyle.width); // 100px
    console.log(computedStyle.height); // 50px
    console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
    console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

    // 상속 스타일(body -> .box)
    console.log(computedStyle.color); // rgb(255, 0, 0)

    // 기본 스타일
    console.log(computedStyle.display); // block
  </script>
</html>
```

getComputedStyle 메서드의 두 번째 인수(pseudo)로 :after, :before와 같은 의사 요소를 지정하는 문자열을 전달할 수 있다.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .box:before {
        content: 'Hello';
      }
    </style>
  </head>
  <body>
    <div class="box">JS Deep Dive</div>
  </body>
  <script>
    const $box = document.querySelector('.box');

    const computedStyle = window.getComputedStyle($box, ':before');
    console.log(computedStyle.content); // "Hello"
  </script>
</html>
```

### 39.9 DOM 표준

2018년 4월부터 구글, 애플, 마이크로소프트, 모질라로 구성된, 4개의 주류 브라우저 벤더사가 주도하는 WHATWG<sup>Web Hypertext Application Technology Working Group</sup>이 단일 표준을 내놓기로 합의했다.

DOM은 현재 다음과 같이 4개의 레벨(버전)이 있다.

| 레벨        | 표준 문서 URL                           |
| ----------- | --------------------------------------- |
| DOM Level 1 | https://www.w3.org/TR/REC-DOM-Level-1/  |
| DOM Level 2 | https://www.w3.org/TR/DOM-Level-2-Core/ |
| DOM Level 3 | https://www.w3.org/TR/DOM-Level-3-Core/ |
| DOM Level 4 | https://dom.spec.whatwg.org/            |
