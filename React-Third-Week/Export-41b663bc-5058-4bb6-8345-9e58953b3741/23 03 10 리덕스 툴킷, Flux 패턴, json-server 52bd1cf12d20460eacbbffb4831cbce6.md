# 23.03.10 리덕스 툴킷, Flux 패턴, json-server

# 리덕스 툴킷

## ConfigureStore

> `configureStore` API 는 인자로 객체를 받으며, `createStore` API 를 대체한다.

인자로 오는 객체 안에는 Reducer 가 `key: reducer` `value: {객체}` 페어로 들어간다.
> 
> - src / redux / config / configStore.js
> 
> ```jsx
> const store = configureStore({
>   reducer: {
>     counter: counter,
>   },
> });
> ```
> 

---

---

## CreateSlice

> `createSlice` 는 인자로 객체를 받으며, 모듈 안의 `Action Creator`, `Reducer` 등을 대체하기 위해 만들어졌다.
인자로 `name`, `initialState`, `reducers` 세개를 받는다.
아래의 세가지를 대체한다.
> 
> 
> ```jsx
> // [slice]
> // 1. export action creator
> // 2. export reducer
> // 3. action value
> ```
> 
> <aside>
> 📌 `name` 에는 state의 이름
> `initialState` 에는 초기값
> `reducers` 에는 보통 여러가지 리듀서가 들어가니 객체 형식으로 `key` 에 actionCreator, `value` 에
> reducer 를 넣어준다.
> 
> </aside>
> 
> 모듈에서 `name` 을 counter 로, `intialState`는 initialState로 줬다.
> 
> 하단의 `export` 에서는 `counterSlice.reducer`로 리듀서를 `export` 해주고,
> 
> `reducers` 내부의 `actions` 를 구조분해할당으로 `export` 해준다.
> 
> ```jsx
> // Initial State
> const initialState = {
>   number: 0,
> };
> 
> const counterSlice = createSlice({
>   name: "counter",
>   initialState,
>   reducers: {
>     addNumber: (state, action) => {
>       state.number = state.number + action.payload;
>     },
>     minusNumber: (state, action) => {
>       state.number = state.number - action.payload;
>     },
>   },
> });
> 
> export default counterSlice.reducer;
> export const { addNumber, minusNumber } = counterSlice.actions;
> ```
> 

## 툴킷에서의 불변성 유지

> 우리는 리덕스에서 불변성을 유지하기 위해 아래와 같이 구조분해 할당 코드를 사용하였다.
> 
> 
> ```jsx
>  return [...state, action.payload];
> ```
> 
> 하지만 redux toolkit 안에 immer 라는 기능이 내장되어 있기 때문에
> 아래의 코드가 가능하다. 그냥 리덕스는 push를 쓰면 불변성 유지가 안되어서
> state 가 바뀐지 알아챌 수 없기 때문에 push를 쓰면 리-렌더링이 안된다.
> 
> ```jsx
> state.push(action.payload);
> ```
> 

```jsx
const todosSlice = createSlice({
  name: "todos",
  initialState,
  reducers: {
    addTodo: (state, action) => {
      // 불변성을 유지하기 위해
      // return [...state, action.payload];
      // redux toolkit 안에 immer 라는 기능이 내장되어 있기 때문에
      // 아래의 코드가 가능하다. 그냥 리덕스는 push를 쓰면 불변성 유지가 안되어서
      // state 가 바뀐지 알아챌 수 없기 때문에 push를 쓰면 리-렌더링이 안된다.
      state.push(action.payload);
    },
    removeTodo: (state, action) => {
      return state.filter((item) => item.id !== action.payload);
    },
    switchTodo: (state, action) => {
      return state.map((item) => {
        if (item.id === action.payload) {
          return { ...item, isDone: !item.isDone };
        } else {
          return item;
        }
      });
    },
  },
});
```

## Flux 패턴

> [Flux 패턴 만화 참고자료](https://bestalign.github.io/translation/cartoon-guide-to-flux/)  🤌🤌
> 

## json-server

- 아주 간단한 DB와 API 서버를 생성해주는 패키지. BE 에서 실제 DB  와 API Server 를 구축하기 전까지 FE 개발에 임시적으로 사용할 mock data 를 생성하기 위함.

## Thunk 맛보기

- 서버에서 가져온 데이터를 전역에서 사용하기 위해서 리덕스와 썽크를 사용한다.

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%201.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%202.png)

reject 시 try catch

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%203.png)

이니셜에 에러와 로딩 필요하다.

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%204.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%205.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%206.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%207.png)

- 리액트 쿼리? - 잘 쓰려면 ‘캐시’ 를 알아야 한다.
    - 서버에서 가져온 todos를 메모리에 담아둔다.
    - 그래서 다시 서버에 요청을 안해도 된다.
    - useEffect 때문에 뭔가에 들어갈 때 마다 서버에서 가져와야 할 정보를 미리 기억해둬서 한번 들어간 곳에 다시 들어갈 때 캐시에 저장된 데이터를 쓸 수 있다. 매번 서버에서 요청을 안해도 된다.

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%208.png)

# 스터디

- payload 묶어줄 수 있다. 시도.
- 수정 넣어줄 때 타이틀, 콘텐트 숨기기???
- 펑션 f2 → 같은애 한번에 변경
- 피그마??
- 피그마 커뮤니티 landing 추천순 보고 연습하기
- 클론코딩 첼린지 → 에어비앤비 - css, 기능성
- 노멀 마켓컬리 - 무한스크롤, 음..

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%209.png)

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
    
    ![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2010.png)
    
    styled component 연습
    
    ![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2011.png)
    
    리액트 jwt 인증, 인가 알아보기. 알아보기만.
    

리덕스 검색만…. 해보기. 리덕스 툴킷도.

커스텀훅

리덕스 검색만…. 해보기. 리덕스 툴킷도.

todoList 수정기능

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2012.png)

function 분리가 가능한 components!

function이름 as 닉네임 으로 가져올 때 이름을 바꿔줄 수 있다.

 * 을 쓰면 파일 안의 모든애들을 가져올 수 있다.

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2013.png)

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2014.png)

React Hooks

![Untitled](23%2003%2010%20%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%90%E1%85%AE%E1%86%AF%E1%84%8F%E1%85%B5%E1%86%BA,%20Flux%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB,%20json-server%2052bd1cf12d20460eacbbffb4831cbce6/Untitled%2015.png)

useInput 찾아보기

- 로직과 뷰를 분리하자. 함수 분리
    - src 폴더에 utils, hook 만들기

변수명, 함수이름 짓는거 중요함. 이름 길어도 ㄱㅊ.