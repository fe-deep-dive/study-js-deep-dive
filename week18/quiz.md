## Quiz

- 진행자 : 서범석
- 날짜 : Dec 12 2023

---

<!--
1. 질문은 이해하기 쉽고 명확하게 적는다.
2. 문제는 아래의 예시를 참고해 작성한다.
3. 문제의 정답은 주석으로 표기한다.
-->

> 1. 아래 코드는 지영의 관통 프로젝트를 React로 바꾼 댓글 기능이다. 구조분해할당을 사용하여 리팩토링 해보자.

```jsx
// props는 다음과 같이 들어온다.
// props: {comment: {
//     { content: 'blah blah', id: 1, user: 'username' }
//   }
// }
const Comments = (props) => {
  return (
    <>
      <span className="username">{props.comment.user}</span>
      <span className="comment-content">{props.comment.content}</span>
    </>
  );
};
```

<!--
답:
const Comments = ({ comment: { user, content } }) => {

  return (
    <>
      <span className='username'>user</span>
      <span className='comment-content'>content</span>
    </>
  );
};
-->

<br>

> 2. 리액트의 useState는 데이터의 변경을 감지해 페이지를 다시 랜더링한다. 여기서 문제가 있는데, 추가 버튼을 눌러도 추가가 되지 않는다. 해결해보자.

```jsx
const Notices = () => {
  const [notices, setNotices] = useState([
    "공지사항1",
    "공지사항2",
    "공지사항3",
    "공지사항4",
  ]);
  const [count, setCount] = useState[5];

  const addNotice = () => {
    // 여기를 작성해보자
  };

  return (
    <div>
      <h1>공지사항</h1>
      {notices.map((notice, index) => (
        <Notice title={notice} id={index} />
      ))}
      <button onClick={addNotice}>추가하기</button>
    </div>
  );
};
```

<!--
답:
const addNotice = () => {
  setNotices(notices => [...notices, '공지사항' + count]);
  setCount(prev => prev + 1);
};
-->
