## Destructuring

### Object Destructuring

- object 내용물을 꺼내어 객체로 선언하여 사용

- ```js
  // object
  const settings = {
      notifications:{
          follow:true,
          alerts:true,
          public:false
      },
      color:{
          theme:"dark"
      }
  }
  
  // 기존 방식 : 필요할 때 마다 상위 객체를 표현해야 함.
  if (settings.notifications.follow) {
      // bla bla
  }
  
  // destructuring : 필요한 객체를 const로 선언 가능
  const {notifications:{follow = false} = {},color} = settings; // follow의 default를 false, notifications의 default를 true로 선언
  console.log(follow) // 출력 : true
  console.log(color) // 출력 : {theme:"dark"}
  ```



### Array Destructuring

- Array의 내용물을 꺼내어 사용. 가져온 데이터를 조작할 필요가 없을 때 사용하면 유용.

- ```js
  const days = ["Mon","Tue","Wen","Thu","Fri","Sat","Sun"]
  
  const [mon,tue,wen, what = "What"] = days; //마찬가지로 default값 선언 가능
  console.log(mon,tue,wen) //출력 : Mon Tue Wen
  ```



### Renaming

- 다른 언어에게 api를 받았을 시 이름을 다시 지어줄 때 주로 사용

- ```js
  const settings = {
      color:{
          chosen_color : "dark"
      }
  }
  
  // 단순 Renaming
  const chosenColor = settings.color.chosen_color || "light"; // 이후 chosenColor 사용
  
  //ES6 Renaming
  const {
      color:{chosen_color:chosenColor = "light"} // default값 선언까지 꼼꼼히
  } = settings;
  
  console.log(chosenColor) // 출력 : dark
  ```

- ```js
  // api 호출 후 변수를 변경할 때
  let chosenColor = blue
  
  ({
      color : chosen_color:chosenColor = "light" // 이미 같은 이름으로 선언된 let 변수를 자동으로 활용
  } = settings);
  ```



### Function Destructuring

- ```js
  function saveSettings({follow,alert,color="blue"}){ // function destructuring
      console.log(settings);
  }
  
  saveSettings({
      follow:ture,
      alert:true,
      mkt:false,
      color:"green"
  });
  ```



### Value Shorthands (변수명 단축)

```js
const follow = checkFollow();
const alert = checkAlert();

const settings = {
    notifications: {
        // 단순 선언
        // follow:follow,
        // alert,alert
        
        // key와 변수명이 같다면 바로 선언 가능
        follow,
        alert
    }
}
```



### Swapping and Skipping

- ```js
  // Swapping
  let mon = "Sat";
  let sat = "Mon";
  
  [sat, mon] = [mon, sat];
  
  // Skipping
  const days = ["Mon","Tue","Wen","Thu","Fri","Sat","Sun"]
  
  const [mon,tue, , , , ,sun] = days; // 콤마만큼 중간 스킵
  
  ```



