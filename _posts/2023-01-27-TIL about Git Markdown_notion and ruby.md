---
title: "TIL: Git, markdown export of Notion and ruby"
excerpt: "깃페이지를 만들면서 배우게 된 깃, 깃허브, 노션 마크다운, 루비 정리"
date: 2023-01-27
categories: Blog
tags: [Blog, jekyll, Github, Git]
---

# TIL about Git Markdown_notion and ruby

- vscode와 github연동에 도움될 사이트 [here] (https://dev-youngjun.tistory.com/7)
## 노션 마크다운 export
- 노션으로 글쓰고 markdown export하면 어떻게 추출이 될지 확인하기 위해 쓰는 글
- 노션에 마크다운 문법이 되어있다. #로 헤딩이 됨
- 생각보다 연동이 잘된다! 이번 포스트도 노션익스포트를 기반으로 앞에 태그만 달아주고 좀만 체크한 것. 

## git push, merge git branch, merge git repository

- local 에서 git push하는 중에 마지막 단계에서 getaddrinfo()에러 발생
    - McFree인지 뭔지 사설 방화벽때문이었다. 삭제했더니 깃푸쉬 잘 됨
    - 노션의 토글은 마크다운에서 그냥 하위항목으로 반영됨. 
- git 엔터프라이즈는 vscode와 연동해 자동 repo 푸쉬가 안됨


### git bash로 local에서 github업로드

- git bash here하고 브랜치도 먼저 확인. config로 이름, 메일 확인
1. git init
2. git add .
3. git status
4. git commit -m “message”
5. git remote add “remote닉네임” (레포.git)
6. git push -u “remote닉넴” +브랜치이름 (예. git push -u origin +main)
- 원래 branch가 다를때 github에서 pull&request로 보고 merge 버튼 눌러서 승인한 뒤 branch를 지우는 방식이 정석. 혼자 작업할 땐 main branch만 사용.
- 
- markdown에서 줄바꿈 할때는 스페이스바 두번

### git repo 합치기(child폴더로)

부모/자식 remote 닉넴: parent/child

1. git remote add parent (git repo부모.햣)
2. git remote add child (git repo자식.git)
3. git subtree add —prefix=(자식 넣을 폴더이름) (부모repo브랜치)
4. git push parent (브랜치)

### Git 작업과 jekyll static은 병렬로 활용가능

### GIT : local과 repo(원격)사이에서 가져오고 내보내고 하는 것

- 항상 쓰는 명령어
    - git branch: 지금 무슨 브랜치로 작업하고 있는건지 체크
        - git checkout (브랜치이름) 브랜치 변경
        - github repo setting에서 branch default를 main에서 master로 변경가능
    - git status: 내 작업들이 잘 올라가고 있는지 체크
    - git remove -v: 원격repo가 닉네임으로 잘 연결되고 있는지 체크
    - repo있을때 수정 루틴:
        1. git pull repo
        2. local에서 작업
        3. git add . (혹은 특정파일만)
        4. git commit -m “message”
        5. git push (remote 닉넴) (브랜치) [e.g. git push origin master]
            - git push할때 some refs~, git pull 쓰라는 에러 나오면 git pull —rebase origin master써서 깃의 원격 저장소와 현재 로컬 저장소를 다시 동기화 시켜주고 다시 git
    - repo에 처음 연결할 때
        1. 작업할 로컬pc 폴더에서 git bash here
        2. git init
        3. git clone ~~
        4. git remote add (repo닉네임) (repo주소)
        5. 이후엔 작업하고 git add., git commit, git push 작업진행

### RUBY : 바로 git page에 반영하기전에, local에서 http가 어떻게 뜨는지 점검해 볼 수 있음 (gem 명령어, bundle로 작업)

- ruby 2.7.7(x86) git작업이므로 32bit, ver2로 설치. 과정 중에 ridk install 체크하고 넘어갈 것
- cmd창에 gem install bundler 입력,
- cmd로 작업 폴더 이동해서 ruby -v로 버전확인, bundle install 입력해서 번들설치
- 로컬 서버 시작해보기: bundle exec jekyll serve로 블로그 서비스 확인
    - (에러발생시) bundle add ~~ 로 없는 파일 추가
- 번외: 깃허브 로컬 작업할때 jekyll 테마 끌어오기
    - jekyll theme를 zip으로 다운받아서 작업폴더에 복사
    
    ![minimal mistake theme 선택복사](https://velog.velcdn.com/images/sugar_ghost/post)
    

#### minimal mistake 테마 수정

- _config.yml 수정
- _data폴더에 navigation.yml수정
- _page 폴더 만들고 뭐 들어갈지 작업
- _post 폴더 만들어서 .md파일 작업, 카테고리, 태그 추가할 것

#### git 작업 설명

- git pull

git pull = git fech + git merge : 원격저장소 커밋과 동기화하고 커밋을 머지시킨다. 원격저장소와 로컬저장소의 상태를 같게 만들기 위해 원격저장소의 소스를 가져오는 것이