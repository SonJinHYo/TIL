# Git이란, Git 시작

## Git

- 분산 **버전 관리** 시스템
- 여러 사람이 한 번에 코드를 관리하기 위한 시스템
  - 한 명이 밀렸을 때, 다른 모두가 기다리지 않을 수 있다
- ![image-20221004115324532](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004115324532.png)
  - 맨 위의 Repository : **git hub**이 담당
  - 개인의 repository : **git**이 담당





## Git 시작하기

1. 현재 디렉토리에서 우클릭으로 git bash를 열고 `git init`을 입력하여, git-repository를 생성

   - ![image-20221004120031676](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004120031676.png)
     - 맨 우측 remote는 git hub
     - Working Directory (unstaged): commit에 해당x 파일
     - Staging Area : 기록(commit)을 남기고 싶은 파일을 Working Directory에서 git add를 통해 저장하는 공간
       - git add : 내가 다음에 commit에 남기고 싶은 것을 남겨놓는다.
     - commit된 파일을 단위로 스냅샷을 찍어 git이 진행된다

2. 해당 폴더에 어떤 파일이나 디렉토리등이 생성되어, 변경사항이 생겼다면, (지금은` ex.py`라 하자)

   1. `git status` 을 입력하여 현재 파일들 상태를 볼 수 있다.
   2. `git add ex.py `  : ex.py를 commit할 파일로 지정
      - **단, add 후 commit된 파일은 다시 Working DIrectory에 존재**
   3. `git status`를 해주면 다시 업데이트 된 상태 확인 가능

3. `git commit -m "add ex.py"`

   - -m :현재 상태를 커밋하면서 뒤의 메세지를 남기겠다. 커밋의 정보를 담아줌.
   - 만약 -m을 하지 않으면, vim 환경으로 이동되어서 vim을 모르면 힘들다

4. `git log`

   - 현재까지 git에 있었던 일들을 확인할 수 있다.

   
