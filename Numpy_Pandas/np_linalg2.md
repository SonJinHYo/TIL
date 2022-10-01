# numpy Linear Algebra 정리2



## 행렬 분해

### `np.linalg.cholesky(A)`

- 행렬 A 의콜레스키 분해 중 **L행렬만**을 반환
- 행렬을 하부,상부로 분해(L,H)
- 분해에 실패할 경우 LinAlgError





### `np.linalg.qr(A)`

- 행렬 A의 QR분해를 리턴한다.

- QR분해 : 행렬 A를 직교행렬,상부삼각행렬로 분해
- 직교행렬 : (A의 전치)(A) = 항등행렬







## 행렬 노름, 행렬식

### `np.linalg.det(A)`

- A의 행렬식을 반환





### `np.linalg.slogdet(A)`

- A의 sign,logdet 값을 반환
- D (행렬식) = sign * exp(logdet)
- A의 원소에 매우 작은 값이나 큰 determinant가 있는 경우에 언더플로우,오버플로우가 생겨날 수 있는데, 이를 방지해줄수 있는 행렬식 계산법





### `np.linalg.Matrix_rank(A)`

- A행렬의 랭크값을 반환





## 행렬 해, 역행렬

### `np.linalg.solve(A,B)`

- **Ax** = **B** 해를 반환





### `np.linalg.lstsq(A,B)`

- **Ax** = **B** 의 최소제곱 해를 반환

- least-squares 줄임말

  





### `np.linalg.inv(A)`

- A의 역행렬 반환
- 역행렬이 존재하지 않는 경우 LinAlgError 일으킴





### `np.linalg.pinv(A)`

- A의 유사 역행렬을 반환





### `np.linalg.tensorinv(A)`

- 다차원 텐서의 역텐서를 반환
