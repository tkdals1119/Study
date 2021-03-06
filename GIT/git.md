# GIT
## 가장 기본이 되는 순서

```
>초기 설정
$ git init // 해당 폴더에서 git 을 사용할 준비를 한다는 뜻

$ git config --global user.email "you@example.com"
or
$ git config --global user.name "Your Name" // 초기에 bash에서 사용자 이메일 혹은 사용자 이름 지정
$ git config --global --list // 현재 설정 정보를 확인할 수 있음

$ git remote add origin 주소 // 원격 저장소와 연결 ( 이름을 꼭 origin 으로 안해도 됨)
$ git checkout -b 브랜치이름 // 브랜치를 생성하고 위치도 그 브랜치로 이동
$ git push origin 브랜치이름 // 생성한 브랜치를 원격저장소에도 push 한다


> 파일 수정 후
$ git add . // 수정 된 파일 뿐만 아니라 모든 파일을 포함 (.점이 그 표시)
$ git commit -m "Message" // commit 메세지와 함께 commit
$ git push origin branch(혹은 마스터) // 원격저장소에 push 할 위치를 정하고 push


> 뭔가 변경될 때마다 변경되는 사이클
 - pull -> commit -> push
```


## 오류
```
> ! [rejected]        master -> master (non-fast forward) // 원격저장소로 push 시도 시 오류 발생

  - 원격저장소에 있는 코드와 로컬저장소에 있는 코드의 버전이 맞지 않아서 발생
  - 원격저장소에 있는 코드를 pull 받아와서 로컬저장소에 있는 코드를 갱신시킨 뒤 push 해야함

  - $ `git pull origin<원격저장소 연결할 당시 생성한 url이름> 브랜치이름` // 로컬 저장소 코드 갱신
  - $ `git push origin 브랜치이름` // 코드를 갱신한 뒤 push 하면 정상적으로 push 된다.
```

```
$ git checkout -t origin/myprofile
> fatal: Cannot update paths and switch to branch 'myprofile' at the same time.
Did you intend to checkout 'origin/myprofile' which can not be resolved as commit ? // 원격저장소에 있는 'myprofile' 브랜치를 받으려고 할 때 오류 발생

  - $ git branch -r // 원격저장소에 있는 브랜치 목록을 확인했을 때 갱신이 안된 것을 확인할 수 있다.
  - $ git remote update // 원격저장소를 갱신시킨다.
  - $ git branch -r // 다시 원격저장소에 있는 브랜치 목록을 확인해보면 갱신이 된 것을 볼 수 있다.
  - $ git checkout -t origin/myprofile // 정상적으로 브랜치가 받아와지는 것을 볼 수 있다.
```

```
$ git push origin develop
error: src refspec develop does not match any.
error: failed to push some refs to '원격저장소 주소' // 원격저장소로 push 시도 시 오류 발생

- 두 가지로 예상해본다
  - commit 을 하지 않고 push 를 했을 경우
     - commit 을 진행하고 push 를 하면 정상적으로 코드가 업로드 된다
  - 로컬저장소에 브랜치가 존재하지 않을 경우, 즉 master만 있을 경우
     - $ git checkout -b 생성할브랜치이름 // 로컬에 있는 master에 브랜치를 하나 생성한 뒤 push 하면 정상적으로 코드가 업로드 된다
```

```
$ fatal: refusing to merge unrelated histories

- $ git merge branch-name --allow-unrelated-histories
  - 이 명령어로 하면 오류는 해결

```
## TIP
```
> fetch && merge && pull의 차이
 - fetch: 원격저장소의 최신 이력을 확인할 수 있다
   - 현재 작업중인 소스들을 변경하는 merge 작업은 하지 않는다.

 - merge: fetch를 하지 않고 merge를 할 경우 그 브랜치의 최신 이력이 포함되지 않은 채 merge가 진행된다

 - pull: 원격저장소의 내용을 가져와 자동으로 merge 작업을 한다.
    - pull = fetch + merge
    - 자동으로 최신이력을 update 한 후(fetch) 병합(merge)를 진행한다

> 당연한 얘기지만 로컬에 있는 빈 브랜치는 원격저장소로 push가 안된다
 - 원격저장소로 push 할 브랜치에서 commit 을 한번 이상 해주고 push 하면 정상적으로 브랜치가 push 되는 것을 볼 수 있다.
```


## 명령어
- #### 원격저장소에서 내려받기
  - `git clone 내려받을곳의주소`

- #### 원격저장소로부터 업데이트 받고 push 하기
  - `git push -u 원격저장소이름(ex:origin) 원격저장소에있는브랜치이름`

- #### 연결 되어 있는 원격저장소 확인
  - `git remote -v`

- #### 연결 되어 있는 원격저장소 삭제
  - `git remote rm 원격저장소이름(ex:origin)`

- #### Git Repository 이름 변경 후 remote URL 수정
  - `git remote set-url 원격저장소이름(ex:origin) http:// url 작성`

- #### 브랜치목록
  - `git branch -r` 원격저장소에 있는 브랜치 목록(r=remote)
  - `git branch -a` 로컬저장소에 있는 브랜치 목록

- #### 브랜치 삭제
  - `git branch -d 브랜치이름` 로컬저장소에 있는 브랜치 삭제 ( -D 강제삭제)
  - `git push origin --delete 브랜치 이름` 원격저장소에 있는 브랜치 삭제

- #### 브랜치 이름 변경
  - `git branch -m 기존브랜치이름 새로운브랜치이름`
    - 원격저장소에 새 브랜치를 추가시키려면 위 명령어로 작업하던 브랜치의 이름을 바꾸고 push 하면 된다.

- #### 원격저장소에 있는 브랜치 받기
  - `git checkout -b 로컬에새로만들브랜치이름 원격저장소이름(ex:origin)/원격저장소에있는브랜치이름`

- #### 원격저장소에 있는 브랜치 받기(원격저장소 브랜치 이름과 동일하게 생성)
  - `git checkout -t 원격저장소이름(ex:origin)/원격저장소에있는브랜치이름`

- #### 추가 된 이력 조회
  - `git log --all`

- #### commit 로그 확인
  - `git log`

- #### commit 합치기
  - `git rebase -i HEAD~4` (최신 4개의 커밋을 하나로 합치기)

- #### 원격 저장소에 있는 코드로 로컬 저장소에 있는 코드 갱신
  - `git pull`

- #### 원격저장소에 있는 특정 브랜치를 pull 받을 때
  - `git pull 원격저장소이름(ex:origin) 브랜치이름`
    - pull 이나 merge 할 때 메세지 적으라고 unix 화면 나올 때가 있는데 그냥 :wq! 하고 나가면됨

- #### 다른 브랜치에 있는 변경 내용을 현재 브랜치와 병합
  - `git merge 가져올브랜치`

- #### merge 되돌리기
  - `git reset --merge ORIG_HEAD`

- #### 변경 내용을 병합하기 전에 어떻게 변했는지 확인
  - `git diff <현재브랜치> <비교 대상 브랜치>`

- #### 현재 코드 상태 & 어떤 파일이 수정 되어 있는지
  - `git status`

- #### commit 취소
  - `git reset HEAD^` 마지막 commit 삭제
  - `git reset --hard HEAD` 마지막 commit 상태로 되돌림

- #### 특정 commit 시점으로 돌아가기
  - `git checkout commit id`
  - 다시 돌아가려면 `git checkout 돌아갈 브랜치 이름` 하면 됨

- #### 변경 사항을 commit 하지 않고 다른 작업을 하고 싶은 경우
  - 보통 add . 까지만 하고 commit 을 하지 않은 채 다른 브랜치로 이동하려 할 경우 commit 을 하고 이동하라는 메세지가 뜸
  - `git stash` 를 하게 되면 일종의 스택개념으로 작업 한 정보가 임시저장 되고 다른 작업 수행. 후에 다시 작업을 진행할 경우
    `git stash pop` 으로 해서 임시 저장해둔 내용을 꺼내면됨

- #### commit message 수정
  - `git commit --amend` 하면 editor 가 나와서 가장 최근의 commit message 를 수정
  - `git commit --amend -m "message"`
  -  과거 commit 의 message 를 수정하려면 수정 할 commit 시점으로 checkout 해서 --amend 하면 됨
