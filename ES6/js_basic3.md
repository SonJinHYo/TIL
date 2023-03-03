## Array

### `Array.of(a,b,...)`

- 입력받은 값들을 가진 array를 반환

### `Array.from(array-like object)`

- 입력받은 array-like object를 array로 반환

### `Array.find(조건)`

- Array를 순차적으로 돌며 조건에 맞는 **첫번째 원소**를 반환. (없을시 -1 반환)
- example : `const target = arr.find(element => element.includes("son"))` : arr 내의 원소 중 `son` 을 포함하는 원소 반환

#### `Array.findIndex(조건)`

- Array를 순차적으로 돌며 조건에 맞는 **첫번째 원소**의 index를 반환 (없을시 -1 반환)

### `Array.fill(a,num1=null,num2=null)`

- Array의 원소를 [num1,num2) 까지 a로 채운다.
- num1,num2 값을 입력하지 않을 시 끝까지로 간주된다.

### `Array.includes(a)`

- Array내에 a가 있는지 bool 값을 반환

