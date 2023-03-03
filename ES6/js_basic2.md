## String

가능한 백틱 방식으로 사용할 것 : template literals

```js
// 비효율 방식
"hi my name is "+name+"!" // +로 이어지고 띄어쓰기 조심해야함

// 백틱 방식
`hi my name is ${name}!`
// 백틱 함수 담기 가능
const add = (a,b) => a+b;
`6+9 is ${add(6,9)}`
```

### HTML fragment

```js
// template literal으로 html 다루기

const addWelcome = () => {
    const div = `
    <div calss = "hello">
	    <h1 class = "title">Hello</h1>
    </div>
    `;
}

const friends = ["a","b","c","d"]
const list = `
	<h1>People i love</h1>
	<ul>
		${friends.map(friend => `<li>${friend}</li>`).join(" ")}
	</ul>
`;

```



### Styled Components

react를 위한 라이브러리, 패킷

```js
const styled = (aElement) => {
    const el = document.createElement(aElement);
    return args => {
        const styles = args[0];
        el.style = styles;
        return el;
    }
}

//함수를 텍스트로 호출
const title = styled("h1")`
	border-radius:10px;
	color:blue;
`;
```



### 유효성 확인 Method

#### `str1.includes(str2)`

- str1 안dp str2가 있는지 bool 값으로 반환

#### `str1.repeat(num)`

- str1 을 num번 반복한 string을 반환

#### `str1.stratsWith(str2)/endsWith(str2)`

- str1이 str2로 시작하는지/끝나는지 bool값으로 반환

