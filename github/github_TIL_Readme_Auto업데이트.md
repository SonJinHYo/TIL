# TIL readme 자동 업데이트 설정

- TIL를 깃허브로 시작하기 위해 이것저것 알아보다가, readme를 자동으로 업데이트 해주는 기능이 있다는 것을 알게되었다. 원래는 그냥 블로그에 쓰고 있었는데 이 글을 보고 이사를 결정했다..!



### repository생성

- 이름은 원하는대로 하고, 단 Readme는 생성해두자
- 처음이라 readme 밖에 없을테지만 문제없다



### build.yml 파일 생성

- .github/workflows/build.yml 파일을 만들어 폴더까지 생성 해준다.

- 그리고 https://github.com/marketplace/actions/til-auto-format-readme  이곳에서 파일을 긁어오는데 아래를 붙여넣어도 된다.

  ``` yaml
  name: Build README
  on:
    push:
      branches:
      - master
      paths-ignore:
      - README.md
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
      - name: Check out repo
        uses: actions/checkout@v2
        with:
          # necessary for github-action-til-autoformat-readme
          fetch-depth: 0
      - name: Autoformat README
        uses: cflynn07/github-action-til-autoformat-readme@1.2.0
        with:
          description: |
            A collection of concrete writeups of small things I learn daily while working
            and researching. My goal is to work in public. I was inspired to start this
            repository after reading Simon Wilson's [hacker new post][1], and he was
            apparently inspired by Josh Branchaud's [TIL collection][2].
          footer: |
            [1]: https://simonwillison.net/2020/Apr/20/self-rewriting-readme/
            [2]: https://github.com/jbranchaud/til
          list_most_recent: 2 # optional, lists most recent TILS below description
          date_format: "2020 Jan 15:04" # optional, must align to https://golang.org/pkg/time/#Time.Format
  ```

- 그대로 붙여넣어주면 된다. **딱 두가지만 바꿔주자**

- 중요)

  - 1. description : Readme 가장 위쪽 문구이다. 없애도 되고 원하는 문구를 넣어준다.

    2. 4번째 줄의 branches의 아래의 **master**을 **main**으로 바꿔주자

       *(이것때매 3시간을 날렸다... 2020년부터 디폴트 branch가 master에서 main바뀌어서 이전에 만들어진 파일에서 나오는 오류라고 한다)*



###  파일 생성

- 이제 repository에 폴더와 파일을 생성하면 readme에 자동으로 참조가 달리게 된다.

