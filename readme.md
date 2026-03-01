Git: 버전 관리 도구 
- 변경된 내용을 관리하는 도구
- 여러 코드를 합치거나 이전 버전으로 돌아가는 등 작업을 할 수 있음

Github: 코드 저장소
: Git을 통해 버전관리가 가능한 원격 저장소.

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
```

# Branch 이해하기

`origin/master`: origin이라는 이름의 remote repo 안에 master branch를 의미.
보통 remote repo를 새로 만들면, 자동으로 가장 초기 상태의 remote repo의 별칭을 origin으로, 초기 상태의 branch는 main으로 만들어진다.
