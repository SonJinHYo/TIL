## Promises

base schema : js는 코드를 순차적으로 실행. 하지만 **비동기성**을 가지기 때문에 이전 코드의 실행이 끝나지 않았더라도 다음 코드를 읽는다.

Promise : 비동기 작업이 맞이할 미래의 완료/실패와 그 결과값을 나타낸다. (따라서 함수를 입력으로 받게 된다)

### Creating Promises

- ```js
  // setTimeout/setInterval(실행함수,대기시간/주기시간,인자)
  
  const amIGood = new Promise((resolve,reject) => {
      // resolve 함수를 실행하여 성공시 결과를 반환.
      setTimeout(resolve,3000,"Yes!")
  })
  
  setInterval(console.log,1000,amIGood); //처음 두 번은 pending, 이후로는 resolve값("Yes!")이 반환
  ```



### Using Promises

- `then`,`catch`을 이용하여 Promise 값 사용

- `then`/`catch`이 실행되면 `catch`/`then`은 실행되지 않는다

 - ```js
   const amIGood = new Promise((resolve,reject) => {
       resolve("Yes!");
       // reject를 이용하여 실패시 결과를 반환.
       reject("Noooooooooooooo~~")
   });
   
   // then
   amIGood.then(value => console.log(value)); // 출력 : Yes!
   
   amIGood
       .then(result => console.log(result)) // 성공시 출력 :Yes
       .catch(error => console.log(error)); // 실패시 출력 : Noooooooooooooo~~
   ```



### Chaning Promises

- ```js
  const amIGood = new Promise((resolve,reject) => {
      resolve(encodedData);
  });
  
  amIGood
      .then(data => decode(data)) // 데이터를 받아 decode 해 주고 (then을 붙여줄 때는 return을 해줘야 다음 then이 받을 수 있다)
      .then(realData => realData) //decode된 데이터를 사용
  	.then....
  	.then....
  	.then....
  	.catch(error => console.log(error)) // 마지막에 catch를 사용하여 이전 모든 then 단계에서 에러 출력 가능
  
  ```



### Promise.all

- 모든 Promise의 Array를 입력값으로 주고, 모든 Promise가 전부 Resolve 되고 나면 각 Promise의 값을 array로 리턴.

- 하나라도 reject가 일어나면 Promise.all도 reject 발생. reject가 발생한 Promise의 reject를 리턴.

- ```js
  const p1 = new Promise((resolve)=> {
      setTimeout(resolve,5000,"First");
  });
  
  const p2 = new Promise((resolve)=> {
      setTimeout(resolve,1000,"Second");
  });
  
  const p3 = new Promise((resolve)=> {
      setTimeout(resolve,3000,"Third");
  });
  
  const motherPromise = Promise.all([p1,p2,p3]);
  
  motherPromise
      .then(value => console.log(value)) // 5초뒤 출력 : ["First","Second","Third"]
  	.catch(error => console.log(error));
  ```



### Promise.race

- Promise.all 과 사용법이 같다.
- 인자의 Promise중 하나만 resolve/reject 되면 Promise.race의 결과도 resolve/reject. (가장 빠른 결과를 사용)



### finally

- Promise의 작업이 끝난 뒤 가장 마지막에 실행

- ```js
  const p1 = new Promise((resolve)=> {
      setTimeout(resolve,10000,"First");
    })
  	.then(value => console.log(value))
  	.catch(err => console.log(err))
  	.finally(()=>console.log("Im done"));
  ```



### 실사용 사례

```js
fetch("http://~~") // fetch는 Promise를 반환
	.then(response => response.json())
	.then(json => {
    	// json으로 작업
    	console.log(json)
	})
	.catch(e => console.log(e));
```

