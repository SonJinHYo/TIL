# React

자바스크립트를 확장한 문법 **JSX**를 통해 property를 HTML 태그의 속성처럼 적을 수 있게 해준다.



## React 장점

- rendering을 최소 단위로 할 수 있다.
- 어플리케이션이  interactive 하도록 만들어주는 라이브러리
- props를 이용해 속성값들을 태그로 전달가능
  - `<Button key={movie.id} ></Button>`
    - Button JSX에 key 변수의 값은 movie.id로 전달





## React

- 두 가지 const를 render 하고 싶을 경우는 div를 만든다

- 현재 폴더에 react App 설치 : `npx create-react-app .`

- type 검사 라이브러리 다운 : `npm i prop-types`

  - 다음 에러 대처법:

    To address all issues (including breaking changes), run:
    npm audit fix --force
    Run `npm audit` for details.

    

    1. yarn 설치 (터미널에 써주세요.)
       npm install --global yarn

    2. 터미널을 적어주세요.
       yarn add prop-types



### function

#### useState(default_value) : 변수와 변수를 제어하는 함수를 반환

- `const [loading, setLoading] = useState(true);` : 
  - loading = true 로 변수 선언, loading을 수정해줄 setLoading 함수 선언
- `setLoading(false);  ` : loading을 false로 수정



#### useEffect(실행할 코드, 변수) : 변수가 바뀔시 코드실행

- `useEffect({hello();},[])` : 변수가 `[]` 일 시 한번만 실행한다는 의미.
- `useEffect({hello();},keyword)` : `keyword`가 수정되면 `hello()` 함수 실행
