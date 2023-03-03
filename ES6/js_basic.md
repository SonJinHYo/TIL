## Variable

### let, const

- var  :  let , const 전에 사용한 변수 선언 

  - 변수 수정을 막지 못한다

- const : 블록 범위의 상수를 선언한다. 상수의 값은 재할당이 안되고 선언도 안된다. overwrite해야 하는 상황이 아닌 이상 const를 쓰는 것이 좋다

- let : var과 같다. 



#### block scope

- `let`, `const` 는 block scope를 가진다. (= 블록 내에서 선언 시 블록 외부에서 접근 불가능, 블록 내부에서 외부로의 접근은 가능)

- `var`은 function scope를 가진다. (=함수의 외부에서의 접근은 차단하지만 if, for 등의 블록 구문에선 외부 접근이 가능)

 

### dead zone

- js는 hoisting을 통해 변수 선언을 끌어올린다.
  - hoisting : 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미. 
    - `var`로 선언한 변수의 경우 `undefined`로 초기화
    - `let`,  `const`로 선언한 변수의 경우 초기화하지 않는다. (에러가 나야할 상황에 에러를 내준다)

###### var은 쓰지말자



## Functions

### arrow functions

- js에서 개선한 함수 모습

- ```js
  name = ["a","b","c"];
  
  // name에 느낌표를 붙이는 함수
  // 기존 함수 모습.    
  const func = name.map(function(item){
      return item+"!"; //map은 항상 return이 있어야 한다
  })
  
  //arrow function
  const gunc = name.map((item)=>{
      return item+!;
  })
  
  // arrow function의 implicit return을 이용하면 간단한 반환시 중괄호를 제외한 내용물만 반환 가능
  const gunc = name.map(item => item+!);
  ```

- **arrow function을 사용하면 안되는 상황**

  - `this` 사용 시, 기존의 function은 해당 객체를 가리키지만 arrow function은 window를 가리키게 된다.
    - eventListener에서의 타겟, object 내부에서 선언된 함수도 마찬가지

- ```js
  // arrow fuinction을 이용한 타겟 찾기
  const arr = ["son@naver.com","jin@gmail.com","hyo@nonono.com","good@gmail.com"];
  
  const foundMail = arr.find((item) => item.includes("@gmail.com")) ;
  console.log(foudMail); // 출력 : jin@gmail.com
  
  // arrow function을 이용한 조건을 만족하는 타겟 어레이 만들기
  const noGmail = arr.filter(item => !item.includes("@gmail.com"));
  console.log(noGmail); // 출력 : ["son@naver.com","hyo@nonono.com"] 
  
  
  // arrow function을 이용한 forEach 활용
  arr.forEach(email => {
  	console.log(email.split("@")[0]);
  }); //출력(각 줄마다):"son","jin","hyo","good"
  ```

### default values

```js
// 기존
function sayHi(name){
    return "Hello "+ (name || "anon")
}
console.log((sayHi("son"))); //출력 : Hello son
console.log((sayHi())); //출력 : Hello anon

// arrow function + default values. 위와 같은 결과
const sayHi = (name = "anon") => "Hello"+name;

```

