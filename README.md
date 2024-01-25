# 깃 커맨드

## 용어 및 개념

* 초기화/기타 커맨드
  - 깃 생성: `git init`
  - 이름/이메일 등록: `git config user.name "이름"`, `git config user.email "이메일"`
  - 상태 보기: `git status`
  - 변경사항 조회: `git diff`
  - 변경 이력 보기: `git log`, `git log --graph`, `gitk`

* 4개의 장소가 존재함
  - Working Directory: Unstaged 상태의 수정된 파일들이 들어 있음. git status로 상태 확인 가능.
  - Staging Area: git add하면 파일은 Staged 상태가 됨. Commit 하기 직전 상태.
  - Local Repository: Commit, 즉 업데이트를 확정한 상태의 파일이 들어 있음. Rollback을 통해 업데이트를 취소할 수 있음.
  - Remote Repository: 위의 3개와 달리 원격지에 있는 저장소.

* 장소 간 이동하기
  - add/reset 커맨드로 변경된 파일을 Staging Area에 올리거나 취소할 수 있다. (`git add .` 또는 `git add <filename>`)
  - commit 커맨드로 저장소에 현재 상태를 확정한다. (`git commit -m "commit message"`)
  - push/pull 커맨드를 이용해 저장소 간 업로드/다운로드가 가능하다.

```
Working      Staging       Local           Remote
Directory    Area (index)  Repository      Repository
|              |              |               |
|------------->|------------->|-------------->|
|     add      |   commit     |      push     |
|              |              |               |
|<-------------|<-------------|<--------------|
|      rm      |    reset     |      pull     |
|              |              |               |
|              |              |               |
```

> 원격 저장소에 로컬 저장소 이력 push하기
`$ git remote add <저장소 이름, 생략하면 origin> <원격 저장소 URL>`

> push하기 (다운로드)
`$ git push <저장소 이름> <브랜치 이름, master 등>`

> 원격 저장소 복제하기
`$ git clone <저장소 이름> <저장할 디렉토리 이름>`

> 원격 저장소 메타정보(커밋, 브랜치, 태그) 다운로드 (로컬 저장소는 변경되지 않음)
`$ git fetch <저장소 이름>`

> push하기 (원격 저장소 복제 이후)
`$ git push <생략>`

> pull하기 (업로드)
`$ git pull <저장소 이름, 생략시 origin> <브랜치 이름, 생략시 master>`

> tag 붙이기
`$ git tag <TAG_NAME>` 또는 `$ git tag -a <TAG_NAME> -m "코멘트"` (태그 정보는 `$ git show {TAG_NAME}`으로 조회 가능)


## 브랜치 관리

* 저장소마다 branch(가지)를 가지고 있다.
  - master 브랜치는 가장 최초로 생기는 브랜치로 origin, main이라고 불리기도 함
  - commit 할 때마다 다음 지점으로 브랜치가 자란다.
  - 사용자가 브랜치를 만들거나 삭제할 수 있다.
  - HEAD 포인터를 이용하여 commit, push, pull이 발생하는 지점으로 선택, 이동할 수 있다.
 
```
  o : master (origin, main라고도 함)
  |
  |       1. `git branch <branchName>` : 새로운 브랜치 생성 (단, 브랜치는 다음 커맨드로 삭제할 수 있음: `git branch -d <branchName>`)
  |-------------------->|
  |                     |
  |                     v
  o (변경 취소)          o : 2. `git checkout <branchName>` : 다른 브랜치로 commit 지점 이동 (다른 commit 지점으로 이동할 수도 있음 - HEAD 이동)
  ^ (이력 삭제)          |
  | `git reset HEAD`    v
  o                     o : 3. `git commit`
  | `git revert HEAD`   |
  v (변경 취소)          |
  o (이력 남김)          |
  |                     |
  o<--------------------|
  |       4. `git checkout master; git merge <branchName>` : 새 브랜치를 master에 합침
  o
  |
```
