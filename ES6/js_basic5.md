## Rest and Spread

### Spread

- 변수를 가져와서 펼치기

 - ```js
   const arr = [1,2,3,4]
   
   console.log(arr) // 출력 : [1,2,3,4]
   
   // spread
   console.log(...arr) // 출력 1 2 3 4
   
   
   // 활용
   const brr = ["a","b",false]
   // arr+brr
   crr = [...arr,...brr]
   
   // object도 가능
   const A = {
       name:"Son",
       age:24
   };
   const B = {
       gender:"male"
   }
   
   const C = {...A,...B} // 합치기
   
   // 추가
   const addArr = [...arr,"D"] // [1,2,3,"D"]
   const addArr2 = [...arr,"D",...arr] // [1,2,3,"D",1,2,3]
   ```

 - 활용법

 - ```js
   const lastNmae = prompt("Last name");
   
   const user = {
       name:"son",
       age:20,
       // spread
       // lastName이 빈 문자열이 아니면 lastName:lastName을 ...으로 전개. 첫 조건이 거짓이라면 lastName이 생기지 않는다
       ...(lastName !== "" && {lastName})
   };
   ```



### Rest

- 받게되는 여러 파라미터를 한 Array에 넣을 때 유용

- ```js
  const func = (firstNum,...another) => {
      console.log(firstNum);
      console.log(another);
  };
  
  func(5,1,7,23,43,1234,123)
  // 출력
  // 5
  // [1,7,23,43,1234,123]
  ```

### Rest + Spread+Destructure

- ```js
  const user = {
      name:"son",
      age:20,
      password:123123
  };
  
  
  // 특정값 제외하기
  // password 제외 함수
  const killPassword = ({password,...rest}) => rest;
  
  
  // default값 설정
  // country default값 설정하며 추가 함수
  const setCountry = ({country="KR",...rest}) => ({country,...rest})
  
  
  // 속성명 바꾸기
  // name을 firstName으로 바꾸기
  const rename = ({name:firstName,...rest}) => ({firstName,...rest})
  ```

