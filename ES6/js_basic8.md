## Async Await

Promise의 업데이트.

- `async`는 함수 앞에 위치. 함수 앞에서 감싸면 해당 함수는 `Promise`를 반환한다.
- `await`는 `Promise`가 반환될 때 까지 대기한다.
- `await`는 `async` 안에서만 사용 가능
- `catch`, `finally`는 파이썬과 비슷한 try 구문 사용

- ```js
  // 기존 Promise
  fetch("http://~~") // fetch는 Promise를 반환
  	.then(response => response.json())
  	.then(json => {
      	// json으로 작업
      	console.log(json);
  	})
  	.catch(e => console.log(e));
  
  
  // async, await
  const getMovies = async() => {
      try{
  	    const response = await fetch("http://~~");
      	const json = await response.json();
  		console.log(json);
      }catch (e){
          console.log(e);
      }finally {
          // ....
      }
  };
  ```



### Pararell Async Await

- `Promise.all` 사용

- ```js
  const getMoviesAsync = async () => {
      try {
          const [moviesResponse, upcomingResponse] = await Promise.all([
              fetch("https://yts.mx/api/v2/list_movies.json"),
              fetch("https://yts.mx/api/v2/movie_suggestions.json?movie_id=100"),
          ]);
  
          const [movies, upcoming] = await Promise.all([
              moviesResponse.json(),
              upcomingResponse.json(),
          ]);
  
          console.log(movies, upcoming);
      } catch (e) {
  	    console.log(e);
      } finally {
    	  console.log("we are done");
      }
  };
  ```

