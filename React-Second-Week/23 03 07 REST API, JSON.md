# 23.03.07

### REST API

- 어떤 자원에 대해 CRUD 를 진행할 수 있게 HTTP Method ( GET, POST, PUT, DELETE ) 를 사용하여 요청을 보내는 것. 이 때, 요청을 위한 자원은 특정한 형태로 표현된다.
- 간단히 말하면 URI 를 통해 정보의 자원을 표현하고, 자원의 행위는 HTTP Method 로 명시한다.
    - 자원 : URI
    - 행위 : HTTP Method
    - 표현
    
    ```jsx
    GET/users/3/profile
    ```
    
    - GET = 행위 그 뒤는 자원!
    - 예시
        
        ```jsx
        // bad
        GET /members/delete/1
        // good
        DELETE /members/1
        
        // bad
        GET /members/show/1
        // good
        GET /members/1
        
        // bad
        GET /members/insert/2
        // good
        POST /members/2
        ```
        
    - 규칙
        
        ```jsx
        http://example.com/posts     (O)
        http://example.com/posts/    (X)
        http://example.com/post      (X)
        http://example.com/get-posts (X)
        --> URI는 명사를 사용하고 소문자로 작성되어야 한다.
        --> 명사는 복수형을 사용한다.
        --> URI의 마지막에는 /를 포함하지 않는다.
        
        http://example.com/post-list  (O)
        http://example.com/post_list  (X)
        --> URI에는 언더바가 아닌 하이픈을 사용한다.
        
        http://example.com/post/assets/example  (O)
        http://example.com/post/assets/example.png  (X)
        --> URI에는 파일의 확장자를 표시하지 않는다.
        ```
        
    
    ## 2. Path Variable vs Query Parameter
    
    - **Path Variable**
        
        > /users/10
        > 
        
        [특징]
        
        - 이름에서도 유추할 수 있듯, 경로 자체에 변수(10)를 사용한 방법입니다.
        - 전체 데이터 또는 특정 하나의 데이터를 다룰 때 처럼, 리소스를 식별하기 위해 사용돼요.
    - **Query Parameter**
        
        > /users?user_id=10
        > 
        
        [특징]
        
        - 데이터를 정렬하거나 필터링 하는 경우 더 적합합니다.
    - **예시를 통해 비교해보기**
        
        ```jsx
        /users # Fetch a list of users     // Path Variable
        /users?occupation=programer # Fetch a list of programer user // Query Parameter
        /users/123 # Fetch a user who has id 123 // Path Variable
        ```
        
    

### JSON 이란

자바스크립트 객체 문법에 토대를 둔, 문자 기반의 데이터 교환 형식

- 예시코드
    
    ```jsx
    {
      "squadName": "Super hero squad",
      "homeTown": "Metro City",
      "formed": 2016,
      "secretBase": "Super tower",
      "active": true,
      "members": [
        {
          "name": "Molecule Man",
          "age": 29,
          "secretIdentity": "Dan Jukes",
          "powers": [
            "Radiation resistance",
            "Turning tiny",
            "Radiation blast"
          ]
        },
        {
          "name": "Madame Uppercut",
          "age": 39,
          "secretIdentity": "Jane Wilson",
          "powers": [
            "Million tonne punch",
            "Damage resistance",
            "Superhuman reflexes"
          ]
        },
        {
          "name": "Eternal Flame",
          "age": 1000000,
          "secretIdentity": "Unknown",
          "powers": [
            "Immortality",
            "Heat Immunity",
            "Inferno",
            "Teleportation",
            "Interdimensional travel"
          ]
        }
      ]
    }
    ```
    

자바스크립트 객체 형태와 완전히 같지는 않아요. 비슷한 것 뿐입니다. 작은 따옴포(’’)가 아닌 큰 따옴표(””)만이 허용됩니다. 

- **메서드**
    
    JSON → 문자열 형태 → **서버 - 클라이언트 간 데이터 전송 시 사용**해요.
    
    하지만, 다음 두 경우를 위해 파싱(parsing) 과정이 필요합니다.
    
    1. JS 객체를 JSON 형태로 전송
    2. JSON 형태를 JS 객체 형태로 사용
    
    **# stringify()**
    
    : 자바스크립트 객체 → JSON 문자열 변환. 네트워크를 통해 객체를 JSON 문자열로 변환할 때 주로 사용합니다.
    
    ```jsx
    console.log(JSON.stringify({ x: 5, y: 6 }));
    // Expected output: "{"x":5,"y":6}"    // key 부분에 "" 큰 따옴표가 있으니 JSON이다.
    
    console.log(JSON.stringify([new Number(3), new String('false'), new Boolean(false)]));
    // Expected output: "[3,"false",false]"
    
    console.log(JSON.stringify({ x: [10, undefined, function(){}, Symbol('')] }));
    // Expected output: "{"x":[10,null,null,null]}"
    
    console.log(JSON.stringify(new Date(2006, 0, 2, 15, 4, 5)));
    // Expected output: ""2006-01-02T15:04:05.000Z""
    
    ```
    
    **# parse()**
    
    : JSON 문자열 → 자바스크립트 객체 변환. 네트워크 통신의 결과를 통해 받아온 JSON 문자열을 프로그램 내부에서 사용하기 위해 JS 객체로 변환할 때 사용.
    
    ```jsx
    const json = '{"result":true, "count":42}';
    const obj = JSON.parse(json);
    
    console.log(obj.count);
    // Expected output: 42
    
    console.log(obj.result);
    // Expected output: true
    ```
    
- 실습 코드
    - `useEffect` 에 의존성 배열을 비워줘서 렌더링됨과 동시에 `fetch` 로 API 를 가져오고, `json` 형식으로 바꿔주고, 바꾼 `json` 을 구조분해할당 하여 `state` 에 넣어준다.
    - `json` 이 들어온 `state` 를 `map` 으로 돌려서 뿌려준다.
    
    ```jsx
    function App() {
      const [data, setData] = useState([]);
    
      useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/posts")
          .then((response) => response.json())
          .then((json) => {
            setData([...json]);
            return console.log(json);
          });
      }, []);
    
      return (
        <div>
          <h3>JSONPlaceholder DATA</h3>
          {data.map((item) => {
            return (
              <div style={{ border: "1px solid black", margin: "10px" }}>
                <ul>
                  <li>{item.userId}</li>
                  <li>{item.id}</li>
                  <li>{item.title}</li>
                  <li>{item.body}</li>
                </ul>
              </div>
            );
          })}
        </div>
      );
    }
    ```
    

# todoList 업그레이드

### styled-components 분리

### Router 로 세부정보 페이지 만들기.

### Redux 로 전역상태관리 하기

## 내일할거

- todos 리덕스 더 쓸 곳 없나 찾아보기
- 라우터로 세부사항 만들기.
- styled-components 로 css 짜기
- 다른 리덕스 만들거 없나 찾아보기.

# 스터디

- payload 묶어줄 수 있다. 시도.
- 수정 넣어줄 때 타이틀, 콘텐트 숨기기???
- 펑션 f2 → 같은애 한번에 변경
- 피그마??
- 피그마 커뮤니티 landing 추천순 보고 연습하기
- 클론코딩 첼린지 → 에어비앤비 - css, 기능성
- 노멀 마켓컬리 - 무한스크롤, 음..

![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled.png)

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
    
    ![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%201.png)
    
    styled component 연습
    
    ![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%202.png)
    
    리액트 jwt 인증, 인가 알아보기. 알아보기만.
    

리덕스 검색만…. 해보기. 리덕스 툴킷도.

커스텀훅

리덕스 검색만…. 해보기. 리덕스 툴킷도.

todoList 수정기능

![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%203.png)

function 분리가 가능한 components!

function이름 as 닉네임 으로 가져올 때 이름을 바꿔줄 수 있다.

 * 을 쓰면 파일 안의 모든애들을 가져올 수 있다.

![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%204.png)

![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%205.png)

React Hooks

![Untitled](23%2003%2007%20108e27539b44468585b9e7b43672f7f2/Untitled%206.png)

useInput 찾아보기

- 로직과 뷰를 분리하자. 함수 분리
    - src 폴더에 utils, hook 만들기

변수명, 함수이름 짓는거 중요함. 이름 길어도 ㄱㅊ.