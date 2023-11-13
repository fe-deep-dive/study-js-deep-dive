# 30장: Date
---
Date는 날짜와 시간을 위한 메서드를 제공한다.

UTC(협정 세계시): 국제 표준시. GMT(그리나치 평균시)라고 부르기도 함. UTC와 GMT는 초의 소수점 단위에서만 차이가 나기 때문에 일상에서는 혼용되어 사용된다. 기술적인 표기에서는 UTC가 사용된다.

KST(한국 표준시): UTC에 9시간을 더한 시간이다. 

현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

## 30.1 Date 생성자 함수

Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가진다. 이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 **밀리초**를 나타낸다.

예를들어 1970년 1월 2일 0시를 나타내는 Date 객체는 내부적으로 정수값 86,400,000(24h * 60m * 60s * 100ms)을 갖는다. 

### Date 생성자 함수로 객체를 생성하는 방법

Date객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다. 다른 날짜와 시간을 다루고 싶은 경우 명시적으로 원하는 날짜와 시간 정보를 인수로 지정한다. Date 생성자 함수로 객체를 생성하는 방법은 4가지가 있다.

**1. new Date()**

인수 없이 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

```jsx
new Date(); // Sun Nov 12 2023 19:12:17 GMT+0900 (한국 표준시)
```

new 연산자 없이 호출하면 Date객체가 아닌 현재 날짜와 시간 정보를 나타내는 문자열을 반환한다.

```jsx
new Date(); // "Sun Nov 12 2023 19:12:17 GMT+0900 (한국 표준시)"
```

**2. new Date(milliseconds)**

숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달한 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)
new Date(86400000); // Fri Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)
```

| time | ms |
| --- | --- |
| 1s | 1000ms |
| 1m | 60,000ms |
| 1h | 3,600,000ms |
| 1d | 86,400,000ms |

**3. new Date(dateString)**

날짜와 시간을 나타내는 문자열을 인수로 전달하면 해당 날짜와 시간을 나타내는 Date 객체를 반환한다. 이때 인수로 전달한 문자열은 Date.parse 메서드로 해석 가능한 형식이어야 한다.

```jsx
new Date('May 26, 2020 10:00:00');
// Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
new Date('2020/03/26/10:00:00');
// Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

**4. new Date(year, month[, day, hour, minute, second, millisecond])**

연, 월, 일, 시, 분, 초, 밀리초 숫자를 전달하면 해당 날짜와 시간을 나타내는 Date 객체를 반환한다. 연, 월은 반드시 지정해야 한다. 지정하지 않은 옵션 정보는 0또는 1로 초기화한다.

인수

| 인수 | 내용 |
| --- | --- |
| year | 1900년 이후의 정수. 0부터 99는 1900부터 1999로 처리된다. |
| month | 월을 나타내는 0~11까지의 정수 |
| day | 일을 나타내는 1~31까지의 정수 |
| hour | 시를 나타내는 0~23까지의 정수 |
| minute | 분을 나타내는 0~59까지의 정수 |
| second | 초를 나타내는 0~59까지의 정수 |
| millisecond | 밀리초를 나타내는 0~999까지의 정수 |

연, 월을 지정하지 않은 경우 1970년 1월 1일 00:00:00(UTC)을 나타내는 Date 객체를 반환한다.

```jsx
new Date(2020, 2);
// Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

new Date(2020, 2, 26, 10, 00, 00, 0);
// Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

// 가독성이 훨씬 좋다.
new Date('2020/3/26/10:00:00:00');
// Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)
```

## Date 메서드

**Date.now**

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```jsx
const now = Date.now(); // 1593971539112
const Date(now); // Mon Jul 06 2020 02:52:19 GMT +0900 (대한민국 표준시)
```

**Date.parse**

**1970년 1월 1일 00:00:00(UTC)을 기점**으로 전달된 지정 시간(new Date(dateString)의 인수와 동일한 형식)까지의 **밀리초**를 숫자로 반환한다.

```jsx
// UTC
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000

// KST
Date.parse('Jan 2, 1970 00:00:00'); // 86400000

// KST
Date.parse('1970/01/02/09:00:00'); // 86400000

```

**Date.UTC**

1970년 1월 1일 00:00:00**(UTC)을 기준으로 인수로 전달된 지정 시간까지의 밀리초를 숫자**로 반환한다. 인수는 로컬타임이 아닌 UTC로 인식된다.

```jsx
// year, month[, day, hour, minute, second, millisecond])형식으로 인수를 전달
Date.UTC(1970, 0, 2); // 8640000
Date.UTC('1970/1/2'); // NaN
```

**Date.prototype.getFullYear**

Date 객체의 연도를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24').getFullYear(); // 2020
```

**Date.prototype.setFullYear**

Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외 월, 일도 설정할 수 있다.

```jsx
const today = new Date();

today.setFullYear(2023);
tody.getFullYear(); // 2023

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear(); // 1900
```

**Date.prototype.getMonth**

Date 객체의 월을 나타낸다. 0(1월) ~ 11(12월) 정수를 반환한다.

```jsx
new Date('2020/07/24').getMonth(); // 6
```

**Date.prototype.setMonth**

Date 객체에 월(0~11)을 설정한다. 월 이외에 일도 설정할 수 있다.

```jsx
const today = new Date();

today.setMonth(0); // 1월
today.getMonth(); // 0
```

**Date.prototype.getDay**

Date 객체의 요일(0~6)을 나타내는 정수를 반환한다. 

```jsx
new Date('2020/07/24'),getDay(); // 5
```

| 요일 | 반환값 |
| --- | --- |
| 일요일 | 0 |
| 월요일 | 1 |
| 화요일 | 2 |
| 수요일 | 3 |
| 목요일 | 4 |
| 금요일 | 5 |
| 토요일 | 6 |

**Date.prototype.getHours**

Date 객체의 시간(0~23)을 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:00').getHours(); // 12 
```

**Date.prototype.setHours**

Date 객체에 시간(0~23) 정수를 설정한다. 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

today.setHours(7);
today.getHours(); // 7

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

**Date.prototype.getMinutes**

Date 객체의 분(0~59)을 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30').getMinutes(); // 30
```

**Date.prototype.setMinutes**

Date 객체에 분(0~59)을 나타내는 정수를 설정한다. 옵션으로 초, 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

// 분 지정
today.setMinutes(50);
today.getMinutes(); // 50

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // 5
```

**Date.prototype.getSeconds**

Date 객체의 초(0~59)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30:10').getSeconds(); // 10
```

**Date.prototype.setSeconds**

Date 객체에 초(0~59)을 나타내는 정수를 설정한다. 옵션으로 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

today.setSeconds(30);
today.getSeconds(); // 30

// 초/밀리초 지정
today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // 10
```

**Date.prototype.getMilliseconds**

Date 객체의 밀리초(0~999)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30:10:150').getMilliseconds(); // 150
```

**Date.prototype.setMilliseconds**

Date 객체의 밀리초(0~999)를 나타내는 정수를 설정한다.

```jsx
const today = new Date();

today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

**Date.prototype.getTime**

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 **밀리초**를 반환한다.

```jsx
new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

**Date.prototype.setTime**

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 **밀리초**를 설정한다.

```jsx
const today = new Date();

today.setTime(86400000); 
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

**Date.prototype.getTimezoneOffset**

UTC와 Date 객체에 지정된 로컬 시간과의 차이를 분 단위로 반환한다. KST는 UTC와 9시간 차이가 나므로 -9h이다.

```jsx
consts today = new Date();

today.getTimezoneOffset() / 60; // -9
```

**Date.prototype.toDateString**

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

**Date.prototype.toTimeSTring**

사람이 읽을 수 있는 형식의 Date 객체의 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // 12:30:00 GMT+0900 (대한민국 표준시)
```

**Date.prototype.toISOString**

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toISOString(); // 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ''); // 20200724

```

- ISO 8601
    
    그레고리력을 사용하는 ISO가 지정한 날짜, 시간 데이터에 대한 표준 규격
    

**Date.prototype.toLocaleString**

인수로 전달한 **로켈을 기준**으로 Date 객체의 **날짜와 시간**을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 **시스템의 로켈**을 적용한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocalString(); // 2020. 7. 24 오후 12:30:00
today.toLocalString('ko-KR'); // 2020. 7. 24 오후 12:30:00
today.toLocalString('en-US'); // 7/24/2020, 12:30:00 PM
today.toLocalString('ja-JP'); // 2020/7/24 12:30:00
```

**Date.prototype.toLocalTimeString**

인수로 전달한 **로켈**을 기준으로 Date객체의 **시간**을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 **시스템의 로켈**을 적용한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocalTimeString(); // 오후 12:30:00
today.toLocalTimeString('ko-KR'); // 오후 12:30:00
today.toLocalTimeString('en-US'); // 12:30:00 PM
today.toLocalTimeString('ja-JP'); // 12:30:00
```

## 30.3 Date를 활용한 시계 예제

현재 날짜와 시간을 초 단위로 반복 출력한다.

```jsx
(function printNow(){
	const today = new Date();
	
	const dayNames = [
		'일요일...',
		'월요일좋아',
		'화요일',
		'수요일',
		'목요일!',
		'금요일!!',
		'토요일!!!'
	];

	const day = dayNames[today.getDay()];

	const year = today.getFullYear();
	const month = today.getMonth()+1;
	const date = today.getDate();
	let hour = today.getHours();
	let minute = today.getMinutes();
	let second = today.getSeconds();
	const ampm = hour >= 12 ? 'PM' : 'AM';

	hour %= 12;
	hour = hour || 12; // hour가 0이면 12를 재할당
	
	minute = minute < 10 ? '0'+minute : minute;
	second = second < 10 ? '0'+second : second;

	const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second}`; 

	console.log(now);
	
	setTimeout(printNow, 1000);
}());
```
