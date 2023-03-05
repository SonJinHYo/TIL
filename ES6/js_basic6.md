

## For of Loop

```js
peoples = ["a","b","c","d","e"];

// 단순 루프
for (let i=0; i<peoples.length; i++){
    console.log(peoples[i]);
}


// For of Loop. 각 원소에 대해 특정 action을 해줄 때
const addQmark = (c,i) => console.log(c+"?",i)  //i는 index를 반환
peoples.forEach(addQmark);
// 출력
//a?
//b?
//c?
//d?
//e?


// for of loop. 위와 같은 결과. 
// forEach와의 차이점 : const,let 선택 가능. array,string,map set 등 모두 사용 가능. 중간 멈춤 가능
for (const people of peoples){
    console.log(people+"?")
}

// 중간 멈춤. d를 찾기 전까지 출력
for (const people of peoples){
    if (people === "d"){
        break;
    }else{
        console.log(people);
    }
}
```

