## Quiz

- 진행자 : 박지영
- 날짜 : Jan 2 2024

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 다음 코드의 실행 결과를 말하시오.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    console.log(document.getElementById('foo').textContent); // 1.

    console.log(document.getElementById('foo').innerHTML);   // 2.
  </script>
</html>
```

<!--
1. Hello world!
#foo 요소 노드의 텍스트를 모두 취득한다. 이때 HTML 마크업은 무시된다.

2. Hello <span>world!</span>
#foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.
-->

<br>

> 2. 불필요한 컨테이너 요소를 추가하지 않고 DOM을 한 번만 변경하여 요소를 추가하세요.

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

    const "변수1" = {1번};

    ['Apple', 'Banana', 'Orange'].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.{2번}(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      "변수1".{2번}($li);
    });

    $fruits.{2번}("변수1");
  </script>
</html>
```

<!--
1. document.createDocumentFragment()
2. appendChild
-->

<br>

> 3. 다음 코드의 실행 이후 $box의 class 어트리뷰트를 말하시오.

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

    $box.classList.replace('red', 'blue');  // 1.
    $box.classList.toggle('foo', true);     // 2.
    $box.classList.toggle('bbu', false);    // 3.
</script>
</html>
```

<!-- 
toggle 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
두 번째 인수로는 불리언 값으로 평가되는 조건식을 전달할 수 있다. 이때 조건식의 평가 결과가 true이면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 추가하고, false이면 제거한다.

1. box blue
2. box blue foo
3. box blue foo
-->
