# numpy Linear Algebra 정리1


## 행렬곱
### `np.dot(A,B,out=None) `

- 두 행렬의 행렬곱을 반환




### `np.linalg.multi_dot(A,*,out=None)`

- 두 개 이상의 행렬곱을 반환
- 자동으로 최적의 계산을 해준다



### `np.vdot(A,B,/)`

- 두 벡터의 복소수 내적까지 계산 가능
- `dot`과는 다르게 복소수까지 계산 가능



### `np.inner(A,B)`

- 두 행렬의 내적 계산
- 복소수 계산의 경우 켤레복소수는 적용 x



### `np.outer(A,B)`

- 두 벡터의 외적 계산



### `np.linalg.matrix_power(A,n)`

- 행렬 A의 n제곱 반환
- A는 정방행렬, n은 정수



### `numpy.kron(A, B)`
- 두 행렬의 크로네커 곱을 반환한다.
- 크로네커 곱 : A의 각 원소에 B행렬을 곱한 결과를 리턴
- A : m x n , B: p x q 이라면, A,B의 크로네커 곱은 mp x nq 사이즈를 가진다



## 행렬의 고윳값, 고유벡터
### `np.linalg.eig(A)` 

- 고윳값 1-D list, 고유벡터 반환
- 고윳값의 인덱스와 고유벡터의 인덱스가 일치하도록 반환

- `np.linalg.eigvals(A)` : 고유벡터만 반환



### `np.linalg.eigh(A)` 

- 복소수 에리미트 행렬 A  or 실수 대칭행렬 A 를 계산할 때 유용

- 고윳값 1-D list, 고유벡터 반환
- 고윳값의 인덱스와 고유벡터의 인덱스가 일치하도록 반환

- `np.linalg.eigvalsh(A)` : 고유벡터만 반환
