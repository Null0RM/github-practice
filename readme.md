Git: 버전 관리 도구 
- 변경된 내용을 관리하는 도구
- 여러 코드를 합치거나 이전 버전으로 돌아가는 등 작업을 할 수 있음

Github: 코드 저장소
- Git을 통해 버전관리가 가능한 원격 저장소.

# Register Personal Info
```bash
git config --global user.name "Null0RM"
git config --global user.email "junghy2301@gmail.com"

git config --list # configuration 정보 확인 
```
# Global Settings
Git에서 기본 브랜치 이름이 `master`로 세팅되면 기본 브랜치를`main`으로 사용하는 프로젝트에서 혼동이 생길 수 있음.
그래서 기본 브랜치 이름을 `main`으로 세팅해준다.

```bash
git config --global init.defaultBranch main 
# 필수적인 작업보다는, 브랜치 이름이 달라서 생기는 오류를 방지하기 위해 한다고 쳐도 좋다.
```

# Basic Usage
```bash
mkdir my-new-project
cd my-new-project

git init # git 초기화
echo "hello world" > README.md # 아무 파일도 없으면 브랜치 생성 안됨
```

# Git Structure
1. Working Directory
2. Stage Area
3. Local Repository
4. Remote Repository 

## Working Directory
작업 디렉토리는 개발하는 프로젝트가 위치하는 공간. 즉, 개발중인 소스파일이 위치해있는 폴더.
-> 버전관리의 대상이 위치하는 공간.
- .git이라는 파일이 생성되는 공간
- commit을 만든다는건 특정 작업 시점에서의 변경사항을 저장한다는 뜻

## Stage Area 
작업 디렉토리와 달리 눈에 명시적으로는 보이지 않지만, commit 후보들이 올라가는 공간. 
- working directory -> stage area로 파일을 올리는걸 `stage에 add한다`고 표현.
- stage에 추가된 파일은 add된 파일이라고 부름.
- 한 번도 add, commit된 적 없는 파일은 `untracked`파일이라 부름

```bash
git add README.md # stage area에 파일을 올리는 명령어
```

working directory에서 stage area로 어떤 파일이 반고되었고, 안되었는지 확인하려면 아래 명령어를 이용할 수 있다. 
```bash
git status
```
- 빨간색: 수정은 했는데 stage area에 add하지 않음
- 초록색: stage area에 add하여 commit할 수 있는 상태


## Repository
Repository에는 버전(commit)들이 기록된다. 즉, add된 파일을 commit하여 새로운 버전을 만들 수 있는데, 이 버전이 올라가는 공간.
commit을 통해 새로운 버전을 만들면 해당 stage영역은 비워진다. 
-m 뒤에는 이 작업이 어떤 내용인지 설명하는 메시지를 적음.

```bash
git commit -m "ADD: README.md" # commit을 통해 새 버전을 Local Repository에 기록
```

이렇게 수정된 repository를 이제는 github(remote repository)와 연결한 후, 작업 내용을 업로드해야 한다. 
git에서 repository를 생성한 뒤, 링크를 복사한다. 
github에서는 주로 main이라는 이름의 브랜치 이름을 사용하기 때문에, main 브랜치로 설정한다.

```bash
git remote add origin "repository link"

git push -u origin main # -u: 앞으로 이 브랜치는 origin의 main으로 보낸다는 옵션.
# 그냥 이번에만 할거면 git push origin main 하면 됨
```

# 협업
## 동기화

**`git fetch`**: remote repository(github) 있는 최신 변경 사항들을 local로 다운로드하는 명령어.
- 이 때, 다운로드한 변경사항을 내 현재 **작업 파일에 반영하지 않음.**
- 코드를 합치기 전에 remote에 어떤 변화가 있는지 미리 살펴볼 수 있음.
```bash
git fetch origin main
# origin에서 main브랜치의 정보를 fetch해 가져옴.
```

**`git merge`**: `git fetch`를 통해 가져온 코드를 내 코드와 합치는 명령어.
- origin/main에 있는 내용과 내 로컬 main의 차이점을 분석해서 하나로 merge.
- 만약 내 로컬에서도 같은 줄을 동시에 수정했으면, **충돌(conflict)**이 발생할 수 있음.
```bash
git merge "branch-name" # 내가 있는 branch로 다른 branch의 내용을 가져와 합침
# main에서 git merge branch_1 : branch_1의 내용이 main에 합쳐짐
git merge --abort # conflict가 발생했을 경우, 합치기 전 상태로 깔끔히 취소
```

**`git pull` = `git fetch`+ `git merge`** 
- 빠르게 작업 수행이 가능하지만, 코드가 꼬였을 때 원인 파악이 복잡할 수 있음.

**`git checkout`**
**작업 위치(focus)를 옮기는 명령어.**
Git은 여러 브랜치를 가지는데, 과거 특정 시점(commit)으로 돌아갈 수도 있음. 
이 떄 `checkout`은 내 작업공간의 파일들을 특정 시점의 status로 변환해주는 역할을 함.

# Branch
처음에 local 저장소와 remote 저장소를 연결할 때, 아래 명령어를 사용하는데, origin 뒤에 주소를 붙여서"앞으로 이 주소를 origin이라고 부르겠다~"라고 하는거라 생각하면 편하다.
```bash
git remote add origin https://github.com/...
```
- origin: remote repository(github)의 주소를 담고있음.
- main: 프로젝트의 branch중 가장 기본이 되는 브랜치.

```bash
git branch # local repo에 있는 branch목록 보여줌. 현재 branch에 *표시.
git branch "new-branch-name" # 새로운 branch 생성. (이동X)
git branch -v # 각 branch의 마지막 commit message 표시.
git branch -a # local뿐 아니라, origin에 있는 branch까지 보여줌

git branch -m "new-name" # 현재 브랜치 이름을 변경.

git branch -d "branch-name" # 작업이 완료된 branch삭제, 합쳐지지 않은 내용 있으면 삭제 막아줌.
git branch -D "branch-name" # 브랜지를 강제로 삭제.
git push origin --delete "branch-name" # remote repo에 있는 branch를 삭제
```

```bash
git switch "branch-name" # 해당 branch로 작업 위치를 옮김
git switch -c "new-branch-name" # branch를 생성, 동시에 해당 branch로 이동
# 과거에는 git checkout을 많이 썼지만, 현재는 switch가 더 명확함.
git switch - # 직전에 머물렀던 브랜치로 돌아감 (브랜치 오갈때 유용)
```

## 사용해보기
```bash
mkdir git-practice
git init

echo "# Profile" >> intro.md
git add intro.md
git commit -m "Initial commit"

git switch -c feature/intro
echo "Name: Null0RM" >> intro.md
git add intro.md
git commit -m "Add name to profile"

cat intro.md # 내용 확인
git switch main(또는 master) 
cat intro.md # 내용 확인

git merge feature/intro
git branch -d feature/intro
```