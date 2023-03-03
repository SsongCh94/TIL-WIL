# 23.03.02 styled components, useState, useEffect, useRef

## JS 로 CSS 를 다루는 방식 - Styled Components, Sass

- Styled Components 는 해당 컴포넌트 내에서만 동작한다.
- 전역 스타일링 : 프로젝트 전체를 아우르는 style ⇒ globally styles
- 프로젝트의 규모가 커지면 공통적으로 사용하는 부분을 빼줄 필요가 있다.
    - 아래의 방식으로 `createGlobalStyle` 을 이용하여 전체 스타일을 꾸며줄 수 있다.
    - `export` 와 `import` 를 해야한다.
    - `<GlobalStyle />` 은 return문 안에 작성해준다.
    
    ```jsx
    import { createGlobalStyle } from "styled-components";
    
    const GlobalStyle = createGlobalStyle`
        body {
            font-family: 'Helvetica', 'Arial', sans-serif;
            line-height: 1.5;
        }
    `
    
    export default GlobalStyle
    ```
    

### Styled Components

- 패키지의 일종
    - JS 코드를 CSS 코드 로 바꿔서 컴포넌트를 꾸미는 방식
    - 해당 컴포넌트 내에서만 쓰인다.
- 터미널에 `yarn add styled-components` 를 입력하여 패키지를 가져온다.
    - package.json에 설치된다.
- 아래와 같은 방식으로 사용한다
    - 변수 선언으로 `StBox` 를 만들어주고, `styled.html태그 ``` 백틱 사이에 css 를 작성한다.
        
        ```jsx
        const StBox = styled.div`
          width: 100px;
          height: 100px;
          border: 1px solid red;
          margin: 20px;
        `;
        
        function App() {
          return <StBox>박스</StBox>;
        }
        ```
        
- 장점
    - 조건적으로 스타일링을 할 수 있다. (조건부 스타일링)
    - JS 방식으로 CSS 를 작성한 것이기 때문에 if, 삼항연산자, switch 문 등을 사용할 수 있다.
- 조건부 스타일링
    - props로 css를 보낼 수 있다! 아래의 문법을 기억하자……
    
    ```jsx
    const StBox = styled.div`
      width: 100px;
      height: 100px;
      border: 1px solid ${(props) => props.borderColor};
      margin: 20px;
      background-color: ${(props) => props.backgroundColor};
    `;
    
    function App() {
      return (
        <>
        <StBox borderColor='red' backgroundColor='beige'>빨간박스</StBox>
        <StBox borderColor='blue' backgroundColor='grey'>파란박스</StBox>
        <StBox borderColor='green' backgroundColor='yellow'>초록박스</StBox>
        </>
      );
    }
    ```
    
    - map 을 이용한 응용
    
    ```jsx
    const StBox = styled.div`
      width: 100px;
      height: 100px;
      border: 1px solid ${(props) => props.borderColor};
      margin: 20px;
    `;
    
    // 박스의 색
    const boxList = ['red', 'blue', 'green', 'black']
    
    // 색을 넣으면, 이름을 반환
    const getBoxName = (color) => {
      switch (color) {
        case 'red':
          return '빨간 박스';
        case 'green':
          return '초록 박스';
        case 'blue':
          return '파란 박스';
        default:
          return '검정 박스';
      }
    }
    
    function App() {
      return (
        <StContainer>
        {
          boxList.map((box) => {
            return <StBox borderColor={box} >{getBoxName(box)}</StBox>
          })
        }
        </StContainer>
      );
    }
    ```
    
    ![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled.png)
    

### Sass

- CSS를 효율적으로 사용하기 위해 만들어진 언어. 코드의 재사용성을 높이고 가독성을 향상시켜주며 Human Error를 줄일 수 있다.
- 변수를 사용하여 자주 쓰는 CSS 코드를 할당해줄 수 있다.
    - 자주 사용하는 컬러 등등
- 중첩해서 쓸 수 있다 (Nesting) ???
- Sass 파일을 임포트하여 쓸 수 있다.
- styled components 내에서 쓰는게 Sass 문법이다.

### default style을 제거하는 방식

- 예를들어 `p` 태그의 경우에는 자체적으로 `margin 16` 을 가지고 있다.
- 이러한 default style을 제거하는 방법.
- CSS RESET 이라고 부른다.
- 아래의 코드를 import 해와서 사용하면 된다.
- CSS RESET 코드
    
    ```jsx
    html, body, div, span, applet, object, iframe,
    h1, h2, h3, h4, h5, h6, p, blockquote, pre,
    a, abbr, acronym, address, big, cite, code,
    del, dfn, em, img, ins, kbd, q, s, samp,
    small, strike, strong, sub, sup, tt, var,
    b, u, i, center,
    dl, dt, dd, ol, ul, li,
    fieldset, form, label, legend,
    table, caption, tbody, tfoot, thead, tr, th, td,
    article, aside, canvas, details, embed, 
    figure, figcaption, footer, header, hgroup, 
    menu, nav, output, ruby, section, summary,
    time, mark, audio, video {
    	margin: 0;
    	padding: 0;
    	border: 0;
    	font-size: 100%;
    	font: inherit;
    	vertical-align: baseline;
    }
    /* HTML5 display-role reset for older browsers */
    article, aside, details, figcaption, figure, 
    footer, header, hgroup, menu, nav, section {
    	display: block;
    }
    body {
    	line-height: 1;
    }
    ol, ul {
    	list-style: none;
    }
    blockquote, q {
    	quotes: none;
    }
    blockquote:before, blockquote:after,
    q:before, q:after {
    	content: '';
    	content: none;
    }
    table {
    	border-collapse: collapse;
    	border-spacing: 0;
    }
    ```
    
- CSS RESET 라이브러리!!!
    
    ![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%201.png)
    

state, useEffect, 최적화! 중요!!

리액트 데브툴 - 프로바이더

## useState 함수형 업데이트 무조건 함수형 쓰자

```jsx
<div>Number State: {number}</div>
    <button onClick={() => {
      // 기존 업데이트 방법
      // setNumber(number + 1)

      //함수형 업데이트 방법
      setNumber((currentNumber)=>{    // currentNumber 가 있는 인자 부분에는 무조건 setState의 state 값이 들어간다.
        return currentNumber + 1;
      })
      }}>누르면 UP</button>
```

- 기존의 방법으로 setNumber 를 3번 해줄 경우 배치성으로 보고, 배치 업데이트가 된다.
    - 배치업데이트 : 한꺼번에 변경된 내용들을 모아서 한번만 반영함
    - 아래의 코드는 `setNumber(number + 1)` 과 똑같다! 3번 동작하지 않는다

```jsx
<div>Number State: {number}</div>
    <button onClick={() => setNumber(number + 1))
		<button onClick={() => setNumber(number + 1))
		<button onClick={() => setNumber(number + 1))
      }}>누르면 UP</button>
```

- 반대로 함수형은 인자로 현재 상태의 state 를 받고, 바꾼 값을 return 하기 때문에 3번 온전히 동작한다.
    - currentNumber 인자가 계속 바뀌기 때문
    - 불필요한 렌더링을 줄이기 위해 배치 업데이트 라는 방식을 채용했다.

```jsx
<div>Number State: {number}</div>
    <button onClick={() => {
        setNumber((currentNumber)=>	currentNumber + 1);
        setNumber((currentNumber)=>	currentNumber + 1);
        setNumber((currentNumber)=>	currentNumber + 1);
      })
      }}>누르면 UP</button>
```

### useEffect

- 렌더링 될 때 마다 특정한 작업을 수행해야할 때 설정하는 훅
    - 컴포넌트가 화면에 보여졌을 때
    - 컴포넌트가 화면에서 사라졌을 때 (return)
- 아래의 코드를 보자.
    1. input 에 값을 입력할 때 마다
    2. value 가 변경된다! state가 변경된다.
    3. state가 바뀌었기 때문에 → App 컴포넌트가 리렌더링된다.
    4. 리렌더링 ⇒ useEffect() 가 실행된다.
    5. input에 뭐라도 입력할 때 마다 1~4 번이 반복된다!
    
    ```jsx
    function App() {
      const [value, setValue] = useState("");
    
      useEffect(() => {
        console.log("hello useEffect!");
      });
    
      return (
        <div>
          <input
            type="text"
            value={value}
            onChange={(event) => {
              setValue(event.target.value);
            }}
          />
        </div>
      );
    }
    ```
    

### 이런 문제를 해결하기 위해 의존성 배열 을 넣어준다.

- 이 배열에 값을 넣으면, 그 값이 바뀔 때에만 useEffect를 실행한다.
- 사용방법은 useEffect가 끝나는 부분에 두 번째 인자로 배열을 넣어준다.
- 아래와 같이 빈 배열 [ ] 을 넣어주면 어떠한 의존하지 않기 때문에 화면이 처음 로딩될 때만 동작한다.

```jsx
useEffect(() => {
    console.log("hello useEffect!");
  },[]);
```

### cleanup 기능

- useEffect는 컴포넌트가 화면에서 사라질 때에도 동작한다! 이걸 cleanup 이라고 부른다.
- 아래와 같이 작성하고, 아래의 return 값에 들어가는 함수가 useEffect가 종료될 때 동작할 것 이다.

```jsx
useEffect(() => {
    console.log(`hello useEffect! : ${value}`);

    return () => {
      console.log('나 사라져요')
    };
  },[value]);
```

### 컴포넌트의 생명주기를 알고있자

- 컴포넌트의 생명주기
    - 컴포넌트가 나타날 때 부터 사라질 때 까지의 주기

![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%202.png)

### useRef

- DOM 요소에 접근할 수 있도록 하는 훅
- 저장공간으로서의 useRef, DOM 요소 접근 방법으로서의 `useRef` 활용 방법을 배운다.

- 아래의 코드를 참고하자. `useState`와 같이 초기값을 설정해줄 수 있다.
- 객체형 `key` 값은 `current`, `value` 값은 ‘초기값’ 으로 나오는걸 볼 수 있다.
- `ref.current` 로 새로운 값을 할당해줄 수 있다.

```jsx
const ref = useRef('초기값');
  console.log('ref', ref);  // 객체형으로 { current : '초기값'} 으로 나온다.

  ref.current = '변경값';
  console.log('ref2', ref); // { current : '변경값'}
```

### 이렇게 설정된 ref 값은 컴포넌트가 계속해서 렌더링 되어도 unmount 전까지 값을 유지한다.

- 저장공간으로써 사용될 수 있다.
    - 변화가 일어나서 리렌더링 되더라도 값을 저장하고 있기 때문.
    - 내부 변수가 초기화되는 것을 막을 수 있다.

### 정리하자면! state 는 리렌더링이 꼭 필요한 값을 다룰 때 쓴다.

### ref 는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.

- 내부적으로 사용되는, 보여지지 않아도 되는 값을 저장할 때

- 아래의 코드에서 `state 증가` 버튼을 누르면 즉시 렌더링이 일어나며 state 값이 올라가는게 실시간으로 화면에 렌더링된다.
- 하지만 `ref 증가` 버튼을 누르면 실제로 `countRef.current` 는 증가하고 있지만, 렌더링은 일어나지 않으니 초기값인 0 이 계속 보여지다가 다른 요소로 인해 렌더링이 일어나는 순간 더해져왔던 값들이 한번에 표시된다.

```jsx
<div style={style}>
        state 영역입니다. {count} <br />
        <button onClick={plusState}>state 증가</button>
      </div>
      <div style={style }>
        ref 영역입니다. {countRef.current} <br />
        <button onClick={plusRef}>ref 증가</button>
      </div>
```

### DOM 관련! 렌더링 되자마자 특정 input이 focusing 돼어야 한다면 useRef를 사용하면 된다.

- 먼저 `렌더링 되자마자` 를 담당하는 `hook`은 `useEffect` 이다.
- `useRef` 를 써서 DOM 을 특정해주고, `useEffect` 로 특정된 돔에 뭘 할건지? 기능을 넣어줄 수 있다. 아래의 코드를 참고하자.
- `input` 태그의 `ref` 값으로 선언된 `useRef` 값을 넣어주고, `useEffect`에서 `idRef.current.focus()` 로 포커스를 넣어줬다.
- 렌더링이 되면 `useEffect` 가 동작하며 `ref` 가 특정한 `input` 태그에 자동으로 포커싱이 될 것이다.

```jsx
const idRef = useRef('');
  const pwRef = useRef('');

  useEffect(() =>{
    idRef.current.focus();    
    // pwRef.current.focus();    
  },[])

  return (
    <>
    <div>
      아이디 : <input type="text" ref={idRef}/>
    </div>
    <div>
      비밀번호 : <input type="password" ref={pwRef}/>
    </div>
    </>
```

### DOM 관련 사용법 응용! 배치업데이트로 인해 생기는 주의사항 추가

- 아래의 코드는 화면이 렌더링 되는 순간 `id input` 으로 포커스가 되고,
- id 값이 10자리가 넘어가는 순간 `pw input` 으로 포커스가 넘어가는 코드이다.
- 주석처리된 주의사항을 읽어보자.

```jsx
const idRef = useRef('');
  const pwRef = useRef('');

  const [id, setId] = useState('');

  //화면이 렌더링 될 때 어떤 작업을 하고싶다 : useEffect
  useEffect(() =>{
    idRef.current.focus();
    if (idRef.current.value.length > 10) pwRef.current.focus(); 

    // console.log(idRef.current.value.length)       
  },[id])

  // onChange 에서 직접적으로 if (id.length >= 10) pwRef.current.focus();
  // 를 넣어줄 수도 있지만, 이렇게 할 시 id.length가 11이 되는 순간
  // pw 로 포커스되는 것을 볼 수 있다.
  // onChange 에서 setId 로 state 값을 바꿔줬지만, 함수 내부에서는 아직
  // state의 변화가 반영되기 전이라 그렇다.
  // 이러한 배치업데이트 방식을 염두에 두고 코드를 짜자.

  return (
    <>
    <div>
      아이디 : <input type="text" ref={idRef} value={id} onChange={(e)=>setId(e.target.value)} />
    </div>
    <div>
      비밀번호 : <input type="password" ref={pwRef}/>
    </div>
    </>
```

### 최적화

### memo

- 컴포넌트 저장기
- 불필요한 렌더링을 막기 위해 `memo` 를 사용해 새로운 컴포넌트만 그리고, 원래 알고있던 요소들은 그대로 그리는 방식으로 최적화를 한다.
- `memo` 로 원래 요소를 기억하는 행위 자체도 성능을 잡아먹기 때문에 가끔은 그냥 다 새로 렌더링하는게 나을 때도 있다!! 상황에 따라 쓰자.
- 대표적으로 무한스크롤 상황에서 `memo` 를 사용한다.

### useCallback

- 함수 저장기
- 함수는 똑같은 동작을 하는 기능이기 때문에 렌더링을 할 때 마다 새로 선언해줄 필요가 없기 때문에! useCallback 을 통해 함수를 기억하고 렌더링을 새로하지 않도록 해준다.
- useState 함수형 업데이트를 쓰는 이유

### useMemo

- 값 저장기
- 역시나 의존성 배열! [ ] 빈 값을 주면 다른 state 들이 바뀌어도 얘는 가만히 있는다.
- [state] 를 넣어주면 state 가 바뀔 때 마다 렌더링하게된다.

- `props` 요소를 통해 원래 있던건지 새로운건지 판별한다.
    - `key` 값을 넣어주는 이유는 `key` 값이 판단에 큰 영향을 미치기 때문이다.
    - 그냥 index 값을 넣어주면 안되는 이유는 다른 map 에서도 중복될 수 있기 때문이다.
    - 형제관계에 있는 컴포넌트끼리만 key가 다르면 된다.

## Redux

- 쉽게 얘기하자면 전역 상태(state) 관리 라이브러리 이다.
- props를 계속해서 내려주며 드릴링할 필요없이 state 를 저장하는 파일을 하나 만들고, 컴포넌트에서 그 파일을 임포트하는 방식으로 파일 내의 state 에 접근할 수 있게 된다.
- 비동기처리 쉽게 가능하다.
- React Query, 리코일  이 두가지 조합해서 쓰는게 요즘 핫함
- 하지만 점유율은 redux 가 제일 높음
- flux패턴 기반! flux 패턴이란 무엇일까.
- 전역상태관리 - context API 와 비슷하다.
- context 는 전역상태관리 툴이 아니다. -
- 구글링 context API, 리덕스 차이점
- context 에서는 미들웨어??? 사용못함, 불변성 유지 가능, 리덕스에선 쓸 수 있음. 비동기통신할때 차이
- 왜 리덕스를 선택했는지에 대해 면접에 잘 나옴!!!!
- 수많은 상태관리 라이브러리중에

## 누적 공부할거

- SPA, MPA 공부,
    
    VUE, ANGULAR 와의 차이점
    
    SEO 공부,
    
    CRUD 배우기
    
    useContext
    
    createContext
    
    useReducer > useState
    
    useState의 상위호환!
    
    todo, setTodo 에서 setTodo가 빠진다.
    
    useState는 컴포넌트 안쪽에서 함수와 state 들을 처리하려 한다. return 위쪽 공간이 지저분해지고, 좋은 코드가 아니다.
    
    reducer 다음으로 reduce
    
    ![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%203.png)
    
    styled component 연습
    
    ![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%204.png)
    
    리액트 jwt 인증, 인가 알아보기. 알아보기만.
    

리덕스 검색만…. 해보기. 리덕스 툴킷도.

커스텀훅

리덕스 검색만…. 해보기. 리덕스 툴킷도.

todoList 수정기능

![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%205.png)

function 분리가 가능한 components!

function이름 as 닉네임 으로 가져올 때 이름을 바꿔줄 수 있다.

 * 을 쓰면 파일 안의 모든애들을 가져올 수 있다.

![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%206.png)

![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%207.png)

React Hooks

![Untitled](23%2003%2002%20styled%20components,%20useState,%20useEffect,%20u%200488caefbb64455a87e1cdaf4005b496/Untitled%208.png)

useInput 찾아보기

- 로직과 뷰를 분리하자. 함수 분리
    - src 폴더에 utils, hook 만들기

변수명, 함수이름 짓는거 중요함. 이름 길어도 ㄱㅊ.