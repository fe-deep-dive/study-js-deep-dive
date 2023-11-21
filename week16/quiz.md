## Quiz

- 진행자 : 박지영
- 날짜 : Nov 21 2023 <!-- e.g. Aug 4 2023 -->

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. `str` 변수에 정규 표현식 `regExp1`과 `regExp2`가 매칭하는지 검색하는 식을 3가지 메서드로 작성하고 결과를 말하시오.

```
const str = 'In my baggy, baggy jeans';
const regExp1 = /baggy/g;
const regExp2 = /BaekYiJin/;
```

<!--
1. RegExp.prototype.exec
regExp1.exec(str); -- ['baggy', index: 6, input: 'In my baggy, baggy jeans', groups: undefined]
regExp2.exec(str); -- null

2. RegExp.prototype.test
regExp1.test(str); -- true
regExp2.test(str); -- false

3. String.prototype.match
str.match(regExp1); -- ['baggy', 'baggy']
str.match(regExp2); -- null
-->

</br>

> 2. 다음 string 메서드의 출력 결과를 말하시오.

```
const str = 'you know what I mean';
// 1.
console.log(str.endsWith('w w', 9));
// 2.
console.log(str.charAt(-3));
// 3.
console.log(str.substring(str.indexOf(' ', 10) + 1, str.length)); 
```

<!--
1. false
endsWith 메서드는 대상 문자열이 첫 번째 인수인 문자열로 끝나는지 확인하여 true 혹은 false를 반환한다. 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.endsWith('w w', 10)을 출력했을 때 true

2. '' 
charAt 메서드는 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

3. 'I mean'
indexOf 메서드는 두 번째 인수로 검색을 시작할 인덱스를 전달하고, 첫 번째 인수로 전달받은 문자열을 검색해서 인덱스를 반환한다.
10번 인덱스부터의 공백 문자열의 위치를 찾고, 그 다음 인덱스부터 문자열의 길이까지의 부분 문자열을 구했으므로 'I mean'을 반환한다.
-->

</br>

> 3. 영문자 대문자, 소문자, 숫자, "-", "_" 로만 구성된 길이 2~10자리 사이의 문자열이 맞는지 검사하는 정규 표현식을 작성하시오.

<!--
/^[A-Za-z0-9_-]{2,10}$/
-->

> [뽀나스 문제] 최소 8자리 이상 영문 대소문자, 숫자, 특수문자가 각각 1개 이상 포함된 문자열이 맞는지 검색하는 정규 표현식을 작성하시오.

<!--
/^(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[#?!@$ %^&*-]).{8,}$/
?=는 전방 탐색 기호로, = 다음에 오는 문자를 찾되, 소비하지 않고 그 앞에 있는 문자를 탐색한다.
-->
