## 44장: REST API

---

REST<sup>REpresentational State Transfer</sup>는 HTTP/1.0과 1.1의 스펙 작성에 참여했고, 아파치 공동 설립자인 로이 필딩<sup>Roy Fielding</sup>의 2000년 논문에서 처음 소개되었다.

REST의 기본 원칙을 성실히 지킨 서비스 디자인을 **RESTful**이라고 표현한다.

REST는 **HTTP 기반, 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처**로, REST API는 **REST를 기반으로 서비스 API를 구현한 것**을 의미한다.

### **44.1 REST API의 구성**

REST API는 자원<sup>resource</sup>, 행위<sup>verb</sup>, 표현<sup>representations</sup>의 3가지 요소로 구성된다.

REST는 자체 표현 구조<sup>self-descriptiveness</sup>로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성요소                       | 내용                           | 표현방법         |
| ------------------------------ | ------------------------------ | ---------------- |
| 자원<sup>resource</sup>        | 자원                           | URI(엔드포인트)  |
| 행위<sup>verb</sup>            | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현<sup>representations</sup> | 자원에 대한 행위의 구체적 내용 | 페이로드         |

### **44.2 REST API 설계 원칙**

- URI는 리소스를 표현하는데 집중한다.

- HTTP 요청 메서드로 행위에 대해 정의한다.

**1. URI는 리소스를 표현해야 한다.**

URI는 리소스를 표현하는 데 중점을 두어야 한다.

```
// bad
GET /getNags/1
GET /nags/show/1

// good
GET /nags/1
```

**2.리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.**

클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법 `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, ...

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

```
// bad
GET /actions/delete/{actionId}

// good
DELETE /actions/{actionId}
```

<br>

### **44.3 JSON Server를 이용한 REST API 실습**

### 44.3.2 db.json 파일 생성

```json
{
  "nags": [
    {
      "id": 1,
      "content": "대학은 어디 갈거니?",
      "price": 50000
    },
    {
      "id": 2,
      "content": "군대는 도대체 언제 갈거니?",
      "price": 50000
    },
    {
      "id": 3,
      "content": "학점(학교 성적)은 잘 나오니?",
      "price": 60000
    }
  ]
}
```

### 44.3.4 GET 요청

```HTML
<html>
<body>
  <pre></pre>
  <script>
    const xhr = new XMLHttpRequest();

    xhr.open('GET', '/nags');
    // xhr.open('GET', '/nags/1');
    xhr.send();

    xhr.onload = () => {
      if (xhr.status == 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.5 POST 요청

```HTML
<html>
<body>
  <pre></pre>
  <script>
    const xhr = new XMLHttpRequest();

    xhr.open('POST', '/nags');
    xhr.setRequestHeader('content-type', 'application/json');
    xhr.send(JSON.stringify({ id: 4, content: '언제까지 그렇게 놀거니?', price: 40000 }));

    xhr.onload = () => {
      if (xhr.status == 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.6 PUT 요청

```HTML
<html>
<body>
  <pre></pre>
  <script>
    const xhr = new XMLHttpRequest();

    xhr.open('PUT', '/nags/4');
    xhr.setRequestHeader('content-type', 'application/json');
    xhr.send(JSON.stringify({ id: 4, content: '취업은 언제쯤 하려고?', price: 100000 }));

    xhr.onload = () => {
      if (xhr.status == 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.7 PATCH 요청

```HTML
<html>
<body>
  <pre></pre>
  <script>
    const xhr = new XMLHttpRequest();

    xhr.open('PATCH', '/nags/4');
    xhr.setRequestHeader('content-type', 'application/json');
    xhr.send(JSON.stringify({ price: Infinity }));

    xhr.onload = () => {
      if (xhr.status == 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.8 DELETE 요청

```HTML
<html>
<body>
  <pre></pre>
  <script>
    const xhr = new XMLHttpRequest();

    xhr.open('DELETE', '/nags/4');
    xhr.send();

    xhr.onload = () => {
      if (xhr.status == 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

<br>
