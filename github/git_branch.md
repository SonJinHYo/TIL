# git brach



## git의 branch

- 코드의 흐름을 분산 - 가지치기한다.
- ![image-20221004121757147](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004121757147.png)
- 왜?
  - 메인 흐름에서 오류가 났을 때, 복사해서 따로 진행
  - 메인 흐름에서 새로운 것을 추가하게 될 때, 따로 나와서 진행 등등
- git에는 기본 branch가 하나 있다. (master branch)



## git branch생성

`git branch -v` : 현재 branch를 확인한다.

1. `git branch develop` : develop branch 생성
   - 생성만 하는 것. 자동으로 branch가 바뀌지 않음
2. `git checkout develop` : develop branch로 이동
3. (수정 작업 진행 후) `git add ex.py` 
   - develop branch에서 commit할 것
4. `git commit -m "modified ex.py"` : develop branch에서 커밋
5. `git checkout master`, `cat ex.py`
   - 두 개의 명령어를 실행해보면, branch의 수정사항이 적용되어 있지 않은 것을 확인할 수 있다.





## git branch 병합하기, 삭제하기

작업중인 branch를 원하는 branch에 병합 가능

- 현재 branch의 작업이 끝나서 master branch에 합쳐줘야 하는 상황 등에 적용

현재 master branch 일 때,



`git merge develop` : develop의 commit을 따라가면서 commit한다.

- master branch가 commit만 따라가므로 develop은 남아있다. 삭제해주기

`git branch -d develop` : develop branch를 삭제한다.



