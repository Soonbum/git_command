# Git

## 프로그램 설치 링크

* [Git](https://git-scm.com/): 윈도우에서도 리눅스 환경처럼 git 명령어를 사용할 수 있는 git bash를 제공함
* [SourceTree](https://www.sourcetreeapp.com/): git 트리를 GUI로 볼 수 있게 해줌
* 리눅스의 경우 다음 명령어로 설치할 수 있음: `sudo apt install git-all`

## 용어 및 개념

* 초기화/기타 커맨드
  - 이름/이메일 등록: `git config --global user.name "이름"`, `git config --global user.email "이메일"`
  - CRLF 속성 설정: `git config --global core.autocrlf true`
  - 기본 브랜치 이름 변경하기: `git config --global init.defaultBranch main`
  - 깃 생성: `git init`
  - 상태 보기: `git status`
  - 변경사항 조회: `git diff`
  - 변경 이력 보기: `git log`, `git log --graph`, `git log --all --decorate --oneline --graph`, `gitk`

* 4개의 장소가 존재함
  - Working Directory: Unstaged 상태의 수정된 파일들이 들어 있음. git status로 상태 확인 가능.
  - Staging Area: git add하면 파일은 Staged 상태가 됨. Commit 하기 직전 상태.
  - Local Repository: Commit, 즉 업데이트를 확정한 상태의 파일이 들어 있음. Rollback을 통해 업데이트를 취소할 수 있음.
  - Remote Repository: 위의 3개와 달리 원격지에 있는 저장소.

* 장소 간 이동하기
  - add/rm 커맨드로 변경된 파일을 Staging Area에 올리거나 취소할 수 있다. (`git add .` 또는 `git add <filename>`)
  - commit 커맨드로 저장소에 현재 상태를 확정한다. (`git commit -m "commit message 1st line" -m "commit message 2nd line" ...`)
  - revert 커맨드는 취소 이력이 남고, reset 커맨드는 취소 이력이 남지 않는다. (`git revert HASH`)
  - push/pull 커맨드를 이용해 저장소 간 업로드/다운로드가 가능하다.

```
Working      Staging       Local                     Remote
Directory    Area (index)  Repository                Repository
|              |              |                         |
|------------->|------------->|------------------------>|
|     add      |   commit     |           push          |
|              |              |                         |
|<-------------|<-------------|<------------------------|
|   restore    |    revert    |           pull          |
|              |   (reset)    |                         |
|              |              |                         |
```

* 원격 저장소 복제하기
  - `$ git clone <저장소 이름> <저장할 디렉토리 이름>`

* 원격 저장소 연결 해제하기
  - `$ git remote remove <저장소 이름>`

* 원격 저장소에 로컬 저장소 이력 push하기 (이후 `git remote -v`로 원격 저장소 주소를 확인할 수 있음)
  - `$ git remote add <저장소 이름> <원격 저장소 URL>`

* push하기 (업로드): 단, 로컬 저장소가 원격 저장소의 최신 내역과 같아야 가능함
  - `$ git push <저장소 이름> <브랜치 이름>` (한 번 하면 다음에는 `git push`만 해도 저장소 이름, 브랜치 이름을 재활용함)

* pull하기 (다운로드)
  - `$ git pull <저장소 이름> <브랜치 이름>` (한 번 하면 다음에는 `git push`만 해도 저장소 이름, 브랜치 이름을 재활용함)

* 원격 저장소 브랜치 삭제
  - `$ git push <저장소 이름> --delete <브랜치 이름>`

* 원격 저장소 변경사항(커밋, 브랜치, 태그) 확인하기 (로컬 저장소는 변경되지 않음)
  - `$ git fetch <저장소 이름>` (`git pull`과 달리 merge/rebase 하지 않음)

* tag 붙이기
  - `$ git tag <TAG_NAME> <HASH>` 또는 `$ git tag -a <TAG_NAME> <HASH> -m "코멘트"` (태그 정보는 `$ git show {TAG_NAME}`으로 조회 가능)

## 깃 예외 처리
  - 최상위 디렉토리에 .gitignore 파일을 넣을 것
  - .gitignore 파일 안에 이력에 포함시키지 않을 파일을 넣는다. (빌드 결과물, 고용량 파일, 보안이 필요한 파일 등)
  ```
  # 모든 .class 파일 무시함
  *.class
  
  # "*.class" 파일에서 lib.class는 예외적으로 이력에 포함시킴
  !lib.class
  
  # 'temp-' 접두사를 가진 모든 json 파일을 무시함
  temp-*.json
  
  # 현재 디렉토리에 있는 build.log 파일만 무시함 (서브디렉토리에 있는 파일은 아님)
  /build.log
  
  # 폴더를 지정할 때는 끝에 슬래시 기호를 넣을 것
  # temp 디렉토리 내 모든 파일을 무시함
  temp/
  
  # doc/notes.txt 파일은 무시하지만, doc/server/arch.txt 파일은 허용함
  doc/*.txt

  # doc/ 디렉토리와 그것의 서브디렉토리 안에 있는 모든 .pdf 파일을 무시함
  # /**는 0개 이상의 디렉토리를 의미함
  doc/**/*.pdf
  ```

## 브랜치 관리

* 저장소마다 branch(가지)를 가지고 있다.
  - master 브랜치는 가장 최초로 생기는 브랜치로 origin, main이라고 불리기도 함
  - commit 할 때마다 다음 지점으로 브랜치가 자란다.
  - 사용자가 브랜치를 만들거나 삭제할 수 있다.
  - HEAD 포인터를 이용하여 commit, push, pull이 발생하는 지점으로 선택, 이동할 수 있다. (HEAD 포인터 3단계 뒤로: `git checkout HEAD^^^` 또는 `git checkout HEAD~3`)
 
```
  o : master (origin, main라고도 함)
  |
  |       1. `git branch <branchName>` : 새로운 브랜치 생성
  |          `git branch --all` : 로컬/원격 저장소의 브랜치를 확인할 수 있음
  |          (단, 브랜치는 다음 커맨드로 삭제할 수 있음: `git branch -d <branchName>`)
  |          (브랜치 이름을 바꿀 경우: `git branch -m <oldBranchName> <newBranchName>`)
  |----------------------->|
  |                        |
  |                        v
  o (변경 취소)            o : 2. `git checkout <branchName>` : 다른 브랜치로 commit 지점 이동 (다른 commit 지점으로 이동할 수도 있음 - HEAD 이동)
  ^ (이력 삭제)            |
  | `git reset HASH`       v
  o                        o : 3. `git commit`
  | `git revert HASH`      |
  v (변경 취소)            |
  o (이력 남김)            |
  |                        |
  o<-----------------------|
  |       4. `git checkout master; git merge <branchName>` : 새 브랜치를 master에 합침
  o
  |
```

## 참고 자료

* [Pro Git Book 한글판 1판](https://dogfeet.github.io/articles/2012/progit.html)
* [Pro Git Book 한글판 2판](https://git-scm.com/book/ko/v2)
* [생활코딩 - Git1](https://www.youtube.com/playlist?list=PLuHgQVnccGMCNJESahrVV-uYGMNYK_vMf)
* [생활코딩 - CLI 버전관리편](https://www.youtube.com/playlist?list=PLuHgQVnccGMATJK16UJ9Fjay0ozrSZKiI)
* [생활코딩 - CLI 브랜치 & 충돌](https://www.youtube.com/playlist?list=PLuHgQVnccGMDU5eAzOz2dZ9KXJF6dkNg3)
* [생활코딩 - CLI 백업](https://www.youtube.com/playlist?list=PLuHgQVnccGMBJr3eVXGvYHDvGNcogEy7v)
* [생활코딩 - CLI 협업](https://www.youtube.com/playlist?list=PLuHgQVnccGMA4LgLoH07e7uEbRbi92Dd2)
* [생활코딩 - cherry-pick & rebase](https://www.youtube.com/playlist?list=PLuHgQVnccGMAb_nOiego7BqfKTRcXsUrB)
* [생활코딩 - Pull request](https://www.youtube.com/playlist?list=PLuHgQVnccGMBXv1OKe3Hn3Jq6F735-uWm)
* [생활코딩 - Visual Studio Code로 다루는 Git](https://www.youtube.com/playlist?list=PLuHgQVnccGMAQvSVKdXFiOo51HUD8iQQm)

# Repo/Manifest

## 용어 및 개념

* 용어
  - Repo: 리포지토리 관리 도구로 다수의 Git 저장소를 관리한다. repo 커맨드는 Python 스크립트로 되어 있으며 단일 커맨드로 여러 개의 저장소를 다운로드할 수도 있다.
  - Manifest: Git 저장소 정보가 명시되어 있는 xml 문서.

* 등장 배경
  - SW 규모와 복잡성 증가, 모듈화 필요성 증대, 다수의 Git 효율적 관리 필요성에 따라 등장하게 되었다.
  - 예를 들면, android L OS의 경우 500개 이상의 Git을 갖고 있다.

## repo 명령어

* repo init: git project list가 명시된 manifest 설정
  - manifest 문서를 관리하는 manifest Git 저장소를 다운로드하고 초기화 작업을 한다.
  - `repo init -u ssh://{계정}@{gerrit inifo}/vw/linux/manifest -b BRANCH [-m MANIFEST.xml] [-g GROUP]`

* repo sync: 설정된 manifest에 명시된 git project를 다운로드
  - repo init 이후에 설정된 manifest 정보를 기반으로 Git 저장소를 로컬에 다운로드한다.
  - `repo sync -c -q --no-tags -j4 [{GIT_PJT1 GIT_PJT2 ... GIT_PJTN}]`
  - `-c`: 현재 지정된 branch 정보만 다운로드(fetch)한다.
  - `-q`: 불필요한 로그는 숨긴다.
  - `-j`: Git 저장소 다운로드시 사용할 쓰레드 개수 지정
  - `--no-tags`: Git 저장소의 tag 정보는 다운로드(fetch)하지 않는다.

* repo start: 모든 git project에 topic branch 생성
  - 현재 원격 브랜치 정보와 동일한 상태로 소스를 변경하고 로컬 브랜치를 생성한다.
  - `repo start BRANCH --all`

* repo status: 모든 git project의 git status 확인

* repo upload: commit을 gerrit에 업로드
  - 로컬에 생성한 commit을 Gerrit에 업로드
  - `repo upload .`

* repo info: git project list 정보 출력

* repo forall: 모든 git project에 대해 git 명령어 실행
  - 다운로드한 Git project에 대해 특정 명령어를 수행할 때 사용한다.
  - `repo forall -c COMMAND -j4`: 모든 Git 저장소에 대해 COMMAND 실행
  - `repo forall GIT_PJT1 GIT_PJT2 ... GIT_PJTN -c COMMAND -j4`: GIT_PROJECT1~N에 해당하는 Git 저장소에 대해 COMMAND 실행
  - `-c`: 뒤의 명령어를 실행한다는 뜻.
  - `-r`: 정규 표현식을 이용하여 특정 Git 저장소에 대해 명령어를 수행함
  - `-i`: -r과 달리 정규 표현식을 이용하여 특정 Git 저장소를 제외하고 명령어를 수행함
  - `-g`: manifest의 특정 group에 해당하는 Git 저장소에 대해 명령어를 수행함
  - `-p`: 결과에 GIt path를 함께 출력함
  - `-j`: repo forall 명령어를 실행할 쓰레드 개수를 지정함

* repo list: 모든 git project에 대한 name, path 속성 출력
  - `-n`: Git 저장소 name만 출력
  - `-p`: Git 저장소 path만 출력
  - `-f`: Git 저장소 full path 출력
  - `-g`: manifest의 특정 group에 해당하는 Git repository 리스트 출력
  - `-r`: 정규표현식에 해당하는 Git 저장소 리스트 출력

* repo manifest: 현재 git project 상태로 manifest 파일 생성
  - 특정 소스 상태를 기록해야 하는 경우, 이 명령어를 실행하여 manifest를 생성함 (예: SW Event 의뢰 시점, OEM 전달 버전, Daily Build 완료 시점 등)
  - `repo manifest -r -o MANIFEST_NAME.xml`


# Gerrit

## 용어 및 개념

* Gerrit은 다양한 코드 리뷰 시스템 중 하나이다.
