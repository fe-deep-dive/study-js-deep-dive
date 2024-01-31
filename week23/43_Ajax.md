## 43장 Ajax
---

### 43.1 Ajax란?

Ajax<sup>Asynchronous JavaScript and XML</sup>란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

Ajax는 브라우저에서 제공하는 Web API인 **XMLHttpRequest 객체**를 기반으로 동작한다. XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/ea8250cb-47d6-4c07-9b8c-e4ae9186e98b"></p>

전통적인 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.

이러한 전통적인 방식은 다음과 같은 단점이 있다.

1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜빡이는 현상이 발생한다.
3. 클라이언트와 서버의 통신이 동기 방식으로 작동하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.

<p align="center"><img src="https://github.com/parkyolo/study-js-deep-dive/assets/39394642/896e9e9f-99cd-4383-95c0-7f78156185d4"></p>

Ajax는 전통적인 방식과 비교했을 때 다음과 같은 장점이 있다.

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

### 43.2 JSON

JSON<sup>JavaScript Object Notation</sup>은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

**43.2.1 JSON 표기 방식**

JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.

```json
{
  "name": "JYP",
  "age": 24,
  "alive": true,
  "hobby": ["study", "sleep"]
}
```

JSON의 키는 반드시 큰따옴표(작음따옴표 사용 불가)로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰따옴표로 묶어야 한다.

**43.2.2 JSON.stringify**

JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다.

클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화<sup>serializing</sup>라 한다.

```jsx
JSON.stringify(value, replacer, space)
```

```jsx
const obj = {
  "name": "JYP",
  "age": 24,
  "alive": true,
  "hobby": ["study", "sleep"]
};

// 객체를 json 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"JYP","age":24,"alive":true,"hobby":["study","sleep"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "JYP",
  "age": 24,
  "alive": true,
  "hobby": [
    "study",
    "sleep"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 변환되지 않는다.
function filter(key, value) {
  return typeof value === 'number' ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "JYP",
  "alive": true,
  "hobby": [
    "study",
    "sleep"
  ]
}
*/
```

JSON.stringify 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.

```jsx
const todos = [
  { id: 41, content: 'timer', completed: true },
  { id: 42, content: 'asynchronous', completed: true },
  { id: 43, content: 'json', completed: false }
];

const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);
/*
string [
  {
    "id": 41,
    "content": "timer",
    "completed": true
  },
  {
    "id": 42,
    "content": "asynchronous",
    "completed": true
  },
  {
    "id": 43,
    "content": "json",
    "completed": false
  }
]
*/
```

**43.2.3 JSON.parse**

JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다.

서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화<sup>deserializing</sup>라 한다.

```jsx
const obj = {
  "name": "JYP",
  "age": 24,
  "alive": true,
  "hobby": ["study", "sleep"]
};

const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: 'JYP', age: 24, alive: true, hobby: Array(2)}
```

배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```jsx
const todos = [
  { id: 41, content: 'timer', completed: true },
  { id: 42, content: 'asynchronous', completed: true },
  { id: 43, content: 'json', completed: false }
];

const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
/*
object [
  {id: 41, content: 'timer', completed: true},
  {id: 42, content: 'asynchronous', completed: true},
  {id: 43, content: 'json', completed: false}
]
*/
```

### 43.3 XMLHttpRequest

자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

**43.3.1 XMLHttpRequest 객체 생성**

XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다. XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

```jsx
const xhr = new XMLHttpRequest();
```

**43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드**

XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공한다. 중요한 프로퍼티와 메서드는 볼드체로 표시했다.

- XMLHttpRequeest 객체의 프로토타입 프로퍼티

  |프로토타입 프로퍼티|설명|
  |---|---|
  |**readyState**|HTTP 요청의 현재 상태를 나타내는 정수. 다음과 같은 XMLHttpRequest의 정적 프로퍼티를 값으로 갖는다.<br>- UNSENT: 0<br>- OPENED: 1<br>- HEADERS_RECEIVED: 2<br>- LOADING: 3<br>- DONE: 4|
  |**status**|HTTP 요청에 대한 응답 상태를 나타내는 정수<br>예) 200|
  |**statusText**|HTTP 요청에 대한 응답 메시지를 나타내는 문자열<br>예) "OK"|
  |**responseType**|HTTP 응답 타입<br>예) document, json, text, blob, arraybuffer|
  |**response**|HTTP 요청에 대한 응답 몸체<sup>response body</sup>, responseType에 따라 타입이 다르다.|
  |responseText|서버가 전송한 HTTP 요청에 대한 응답 문자열|

- XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

  |이벤트 핸들러 프로퍼티|설명|
  |---|---|
  |**onreadystatechange**|readyState 프로퍼티 값이 변경된 경우|
  |onloadstart|HTTP 요청에 대한 응답을 받기 시작한 경우|
  |onprogress|HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|
  |onabort|abort 메서드에 의해 HTTP 요청이 중단된 경우|
  |**onerror**|HTTP 요청에 에러가 발생한 경우|
  |**onload**|HTTP 요청이 성공적으로 완료한 경우|
  |ontimeout|HTTP 요청이 초과한 경우|
  |onloadend|HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패하면 발생

- XMLHttpRequest 객체의 메서드

  |메서드|설명|
  |---|---|
  |**open**|HTTP 요청 초기화|
  |**send**|HTTP 요청 전송|
  |**abort**|이미 전송된 HTTP 요청 중단|
  |**setRequestHeader**|특정 HTTP 요청 헤더의 값을 설정|
  |getResponseHeader|특정 HTTP 요청 헤더의 값을 문자열로 변환|

- XMLHttpRequest 객체의 정적 프로퍼티

  |정적 프로퍼티|값|설명|
  |---|---|---|
  |UNSENT|0|open 메서드 호출 이전|
  |OPENED|1|open 메서드 호출 이후|
  |HEADERS_RECEIVED|2|send 메서드 호출 이후|
  |LOADING|3|서버 응답 중(응답 데이터 미완성 상태)|
  |**DONE**|4|서버 응답 완료|

**43.3.3 HTTP 요청 전송**

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

- XMLHttpRequest.prototype.open

  open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다.
  
  ```jsx
  xhr.open(method, url[, async])
  ```
  
  |매개변수|설명|
  |---|---|
  |method|HTTP 요청 메서드("GET", "POST", "PUT", "DELETE" 등)|
  |url|HTTP 요청을 전송할 URL|
  |async|비동기 요청 여부. 옵션으로 기본값은 true이며, 비동기 방식으로 동작한다.|
  
  HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)를 알리는 방법이다. 주로 5가지 요청 메서드를 사용하여 CURD를 구현한다.
  
  |HTTP 요청 메서드|종류|목적|페이로드|
  |---|---|---|---|
  |GET|index/retrieve|모든/특정 리소스 취득|X|
  |POST|create|리소스 생성|O|
  |PUT|replace|리소스의 전체 교체|O|
  |PATCH|modify|리소스의 일부 수정|O|
  |DELETE|delete|모든/특정 리소스 삭제|X|
  
  <details>
  <summary>페이로드</summary>
  페이로드는 전송의 근본적인 목적이 되는 데이터의 일부분으로 그 데이터와 함께 전송되는 헤더, 메타데이터와 같은 부분은 제외한다.
  
  프로그래밍에서 주로 메시지 프로토콜<sup>message protocols</sup> 중에 프로토콜 오버헤드<sup>protocol overhead</sup>와 원하는 데이터를 구별할 때 사용된다.
  
  예를 들어 웹 서비스 응답(web service response)이 아래의 JSON이라 하면:
  
  ```json
  {
      "status":"OK",
      "data": {
          "message":"Hello, world!"
      }
  }
  ```
  
  여기서 "Hello, world!"가 클라이언트가 관심을 가지는 페이로드이다.
  </details>

- XMLHTTPRequest.prototype.send

  send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.
  
  - GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열<sup>query string</sup>로 서버에 전송한다.
  - POST 요청 메서드의 경우 데이터(페이로드<sup>payload</sup>)를 요청 몸체<sup>request body</sup>에 담아 전송한다.
  
  페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달해야 한다.
  
  ```jsx
  xhr.send(JSON.stringify({id: 1, content: 'HTML', completed: false }));
  ```
  
  HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달된 인수는 무시되고 요청 몸체는 null로 설정된다.

- XMLHttpRequest.prototype.setRequestHeader

  setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야 한다.
  
  자주 사용하는 HTTP 요청 헤더인 Content-type과 Accept에 대해 살펴보자.
  
  Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다. 자주 사용되는 MIME 타입은 다음과 같다.
  
  |MIME 타입|서브타입|
  |---|---|
  |text|text/plain, text/html, text/css, text/javascript|
  |application|appliation/json, application/x-www-form-urlencode|
  |multipart|multipart/formed-data|
  
  <details>
    <summary>MIME 타입</summary>
  MIME 타입<sup>Multipurpose Internet Mail Extensions</sup>이란 문서, 파일 또는 바이트 집합의 성격과 형식을 나타낸다.
    
  MIME 타입은 가장 일반적으로 슬래시(/)로 구분된 'type'과 'subtype'의 두 부분으로 구성된다.
  
  `type/subtype`
  
  type은 video 또는 text와 같이 데이터 타입이 속하는 일반 카테고리를 나타낸다.
  
  subtype은 MIME 타입이 나타내는 지정된 타입의 정확한 데이터 종류를 식별한다. 예를 들어, MIME 타입 text의 경우 하위 타입은 plain (평문 텍스트), html (HTML 소스 코드) 또는 calendar (iCalendar/.ics 용)이다.
  
  각 타입에는 고유한 하위 타입이 있다. MIME 타입은 항상 타입과 하위 타입 모두 가지며, 둘 중 하나만 가지지는 않는다.
  </details>
  
  다음은 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정하는 예다.
  
  ```jsx
  // XMLHttpRequest 객체 생성
  const xhr = new XMLHttpRequest();
  
  // HTTP 요청 초기화
  xhr.open('POST', '/users');
  
  // HTTP 요청 헤더 설정
  // 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
  xhr.setRequestHeader('content-type', 'application/json');
  
  // HTTP 요청 전송
  xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
  ```
  
  HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있다.
  
  다음은 서버가 응답할 데이터의 MIME 타입을 지정하는 예다.
  
  ```jsx
  // 서버가 응답할 데이터의 MIME 타입 지정: json
  xhr.setRequestHeader('accept', 'application/json');
  ```
  
  만약 Accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 Accept 헤더가 */*으로 전송된다.

**43.3.4 HTTP 응답 처리**

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.

XMLHttpRequest 객체가 갖는 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 매치하여 다음과 같이 HTTP 응답을 처리할 수 있다.

참고로 HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다. 다음 예제에서는 JSONPlaceholder에서 제공하는 가상 REST API를 사용한다.

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않은 상태다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed:false}
  } else {
    console.log('Error', xhr.status, xhr.statusText);
  }
};
```

readystatechange 이벤트 대신 load 이벤트를 캐치해도 좋다.

load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다. 따라서 load 이벤트를 캐치하는 경우 xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요가 없다.

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", complted:false}
  } else {
    console.log('Error', xhr.status, xhr.statusText);
  }
};
```
