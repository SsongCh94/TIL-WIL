# 23.03.05 Hooks, Virtual DOM, 동기 비동기

<aside>
💡 Q2. 리액트 개발 시, 렌더링 최적화 방법에 이용할 수 있는 다음 세 가지의 특징과 차별점을 각각 말씀해주세요.

1. React.memo
2. useCallback
3. useMemo
</aside>

1. **`React.memo`**
    - 함수형 컴포넌트의 렌더링을 최적화하는 데 사용됩니다.
    - 이전에 렌더링된 결과를 기억하고, 이전에 전달된 props가 같은 경우에는 다시 렌더링하지 않습니다.
    - 렌더링 속도를 높이고, 불필요한 렌더링을 방지하여 애플리케이션의 성능을 개선합니다.
2. **`useCallback`**
    - 함수를 기억하여 같은 함수가 불필요하게 재생성되는 것을 방지합니다.
    - 의존성 배열(dependency array)이 변경될 때에만 함수를 재생성합니다.
    - 이전에 생성된 함수를 재사용하여 렌더링 속도를 높이고, 애플리케이션의 성능을 개선합니다.
3. **`useMemo`**
    - 계산 비용이 높은 함수의 반환 값을 기억하여 렌더링 속도를 높이는 데 사용됩니다.
    - 의존성 배열(dependency array)이 변경될 때에만 값을 다시 계산합니다.
    - 기억된 값이 변경되지 않으면 이전에 계산된 값을 재사용하여 렌더링 속도를 높이고, 애플리케이션의 성능을 개선합니다.
    - 

따라서, **`React.memo`**, **`useCallback`**, **`useMemo`**은 모두 함수의 결과를 기억하여 불필요한 렌더링을 방지하고, 렌더링 속도를 높이는 데 사용됩니다. 그러나 각각의 특징은 다르기 때문에, 어떤 상황에서 어떤 최적화 방법을 사용할지 결정할 때는 각각의 특징을 고려해야 합니다.

<aside>
💡 Q5. React Hook 중 useRef는 어느 경우 사용하게 되나요? 두 가지 이상 적어봅시다.

</aside>

### 저장공간으로의 활용

```jsx
const ref = useRef('초기값');
  console.log('ref', ref);  // 객체형으로 { current : '초기값'} 으로 나온다.

  ref.current = '변경값';
  console.log('ref2', ref); // { current : '변경값'}
```

### 위와 같이 설정된 ref 값은 컴포넌트가 계속해서 렌더링 되어도 unmount 전까지 값을 유지한다.

- 저장공간으로써 사용될 수 있다.
    - 변화가 일어나서 리렌더링 되더라도 값을 저장하고 있기 때문.
    - 내부 변수가 초기화되는 것을 막을 수 있다.

### useRef 는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.

- 내부적으로 사용되는, 보여지지 않아도 되는 값을 저장할 때 사용된다.

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

### DOM 요소에 대한 참조 생성

- DOM 요소에 대한 참조를 current 프로퍼티에 저장할 수 있다.
- 이를 통해 DOM 요소를 직접 조작하거나, 다른 Hook과 함께 사용할 수 있다.

```jsx
function Component() {
  const myRef = useRef(null);

  function clickHandler() {
    myRef.current.focus();
  }

  return (
    <div>
      <input type="text" ref={myRef} />
      <button onClick={clickHandler}>Focus</button>
    </div>
  );
}
```

<aside>
💡 Q6. 리액트가 왜 Virtual DOM을 통해 컴포넌트를 그리는 것일까요? Virtual DOM을 사용할 때 왜 더 효율적인지에 대해 설명해주세요.

</aside>

- 실제 DOM 을 조작하는 것은 매우 소모적인 일이다.
    - 소모적인 이유
        - 실제 DOM은 아래와 같은 과정을 거쳐 렌더링이 일어난다.
        1. DOM tree 생성
        2. render tree 생성
        3. Layout
        4. Paint
        - 문제는 DOM에 어떤 변화가 발생하면 렌더링 과정은 render tree 부터 다시 재시작된다.
        - 심지어 최근에는 SPA 가 유행하며 DOM tree를 즉각적으로 변경할 일이 많아졌다.
- Virtual DOM 은 실제 DOM 과 같은 속성들을 지녔지만, API 는 없다. (getElementById 등등)
- Virtual DOM 은 html 객체에 기반한 자바스크립트 객체의 형태로 메모리에 저장된다. 그리고 이러한 처리는 실제 DOM 이 아닌 메모리 상에서 동작하기 때문에 훨씬 더 빠르게 동작한다. 그리고 실제로 렌더링하지 않기 때문에 연산 비용이 아주 적다. 요소가 30개가 바뀌어도 30번 렌더링하는게 아니라, 모든 변화를 하나로 묶어서 딱 한 번만 실행시킨다. (배치 업데이트)
- virtual DOM 이 하는 일은 DOM 의 변화를 묶어서 적용한 뒤 실제 DOM에 던져주는 일이다.
- 데이터가 변경되면 전체 UI는 virtual DOM에 렌더링된다. 그리고 이전 virtual DOM 과 비교하여 바뀐 부분만 실제 DOM 에 적용시킨다.
- 하지만 만약 정보 제공만 하는 웹페이지라면 변화가 발생하지 않기 때문에 일반 DOM의 성능이 더 좋을 수도 있다.

<aside>
💡 Q7. 비동기 프로그래밍이란 무엇인가요? 또한 콜백지옥이 발생할 때의 문제점은 무엇이며 이를 극복하기 위해 대안으로 나온 방법 2가지를 설명해주세요.

</aside>

### 비동기 프로그래밍

- 비동기 프로그래밍이란 어떤 작업을 수행할 때 그 결과가 나타날 때까지 기다리지 않고, 다음 작업을 수행하는 프로그래밍 방식이다. 이를 통해 더욱 효율적인 프로그램을 작성할 수 있다.

### 콜백지옥의 문제점과 해결방안

- 여러 개의 비동기적인 작업을 수행하다 보면, 각 작업이 완료될 때마다 콜백함수를 실행하는데, 이러한 콜백함수가 중첩되어 코드가 복잡해지면 가독성이 떨어지고 디버깅이 어렵다.
- 해결방안
    - `promise`
        - `Promise`는 비동기 작업의 결과를 나타내는 객체이다.
        - `Promise`는 성공, 또는 실패 상태를 가질 수 있으며, 이를 처리하는 메서드인 `then` 과 `catch`를 제공한다.
        - 아래의 함수 `fetch` 는 서버에서 데이터를 가져오는 비동기 작업을 하고, 작업이 완료되면 `then` 으로 응답을 json 화 시켜준 뒤 출력한다. 만일 작업이 실패하면 `catch` 가 호출된다.
        
        ```jsx
        fetch('https://api.example.com/data')
          .then(response => response.json())
          .then(data => console.log(data))
          .catch(error => console.error(error));
        ```
        
    - `async/await`
        - `Promise` 를 조금 더 쉽게 사용할 수 있도록 만들어진 기능이다.
        - 비동기적인 작업을 동기적인 코드처럼 작성할 수 있다.
        - 아래의 코드는 `async` 코드이다.
            - `try` 와 `catch` 는 예외가 발생할 시 처리하는 코드이다.
        
        ```jsx
        async function fetchData() {
          try {
            const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
            const data = await response.json();
            console.log(data);
          } catch (error) {
            console.log(error);
          }
        }
        ```
        
        - 하지만 `promise` 의 경우 `promise` 체인 중간중간 다른 작업을 수행하는게 가능하다는 점 때문에 `promise` 를 사용해야 하는 경우도 있다.
            - 아래는 `promise` 체인 도중 ui를 업데이트해주는 코드이다.
        
        ```jsx
        function updateUI(data) {
          // UI 업데이트 작업 수행
          console.log(data);
        }
        
        fetch('https://jsonplaceholder.typicode.com/todos/1')
          .then(response => response.json())
          .then(data => {
            updateUI(data); // 첫 번째 Promise 객체 처리 후 UI 업데이트
            return fetch('https://jsonplaceholder.typicode.com/todos/2');
          })
          .then(response => response.json())
          .then(data => {
            updateUI(data); // 두 번째 Promise 객체 처리 후 UI 업데이트
          })
          .catch(error => console.log(error));
        ```
        

## Virtual DOM

React는 Virtual DOM을 사용하여 컴포넌트를 그립니다. Virtual DOM은 실제 DOM을 추상화한 가상의 객체 모델입니다. 실제 DOM과는 달리 메모리 상에만 존재하며, 빠르게 업데이트할 수 있습니다.

- React에서는 `state`가 변경될 때마다 전체 컴포넌트를 다시 렌더링합니다. 그러나, 이 과정에서 실제 DOM을 조작하는 것은 비용이 많이 듭니다.
- Virtual DOM은 이러한 문제를 해결하기 위해, 실제 DOM 조작을 최소화하면서 컴포넌트를 업데이트합니다. 변경된 부분만 추적하고, 실제 DOM 조작은 최소화합니다. diffing 과 reconciliation.

### props

- props는 컴포넌트 간 데이터를 전달하기 위한 방법 중 하나이다.
- 각 컴포넌트는 다른 컴포넌트에서 전달된 props를 사용할 수 있다.
- props 는 부모 컴포넌트에서 자식 컴포넌트로 전달되며 이를 통해 부모 컴포넌트는 자식 컴포넌트에 데이터를 전달할 수 있다.
- props 는 읽기전용 이며 부모 컴포넌트에서 자식 컴포넌트로 전달된 props는 자식 컴포넌트에서 직접 수정할 수없다.
- props 는 객체 형태로 전달된다. props 를 받는 자식 객체에서는 아래와 같이 props를 사용할 수도 있다.

```jsx
function App() {
  return <Child message={"Hi"} />;
}

function Child(props) {
  return <div>{props.message}</div>;
}
```

- 이를 통해 리액트에서는 각 컴포넌트를 독립적으로 작성하고, 재사용성을 높일 수 있다.

### state

- state는 컴포넌트 내부에서 관리되는 값으로, 컴포넌트가 동작하는 동안 값이 변할 수 있다.
- `useState` Hook 을 통해 만들 수 있다.
- 컴포넌트가 화면에 그려질 때마다 업데이트되며, state 가 업데이트되면 리액트는 변경된 부분만 리-렌더링 한다.
- state는 컴포넌트 내부에서만 사용할 수 있다. state를 다른 컴포넌트와 공유하려면 props나 전역 상태 관리 라이브러리인 Redux 등을 이용해야 한다.

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
    
    ![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled.png)
    
    styled component 연습
    
    ![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled%201.png)
    
    리액트 jwt 인증, 인가 알아보기. 알아보기만.
    

리덕스 검색만…. 해보기. 리덕스 툴킷도.

커스텀훅

리덕스 검색만…. 해보기. 리덕스 툴킷도.

todoList 수정기능

![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled%202.png)

function 분리가 가능한 components!

function이름 as 닉네임 으로 가져올 때 이름을 바꿔줄 수 있다.

 * 을 쓰면 파일 안의 모든애들을 가져올 수 있다.

![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled%203.png)

![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled%204.png)

React Hooks

![Untitled](23%2003%2005%20Hooks,%20Virtual%20DOM,%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%20%E1%84%87%E1%85%B5%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%203fa593c2412443c49444d10dc8bb11ab/Untitled%205.png)

useInput 찾아보기

- 로직과 뷰를 분리하자. 함수 분리
    - src 폴더에 utils, hook 만들기

변수명, 함수이름 짓는거 중요함. 이름 길어도 ㄱㅊ.