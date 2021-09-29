# Git

Git은 2005년 Linux OS 커널을 만든 Llinus Torvalds가 개발한 오픈 소스 프로젝트이자 전세계적으로 가장 널리 사용되는 버전 관리 시스템이다. 

Git은 다른 버전 관리 시스템들과 비교해 뛰어난 성능, 보안, 유연성을 가지며 다양한 OS 및 IDE에서 사용할 수 있다. 또한 코드 버전을 관리함으로써 개발 워크플로를 효율적으로 개선할 수 있다.

<br>

## Git Commands

### Basic

- git init : git 생성
- git clone git_path : 코드 가져오기
- git add file_path : 수정한 코드 선택하기 ( git add * )
- git commit -m “commit_description” : 커밋 설명과 함께 커밋
- git push romote_name branch_name : add하고 commit한 코드 git server에 보내기 (git push origin master)
- git pull : git서버에서 최신 코드 받아와 merge 하기
- git fetch : git서버에서 최신 코드 받아오기
- git reset — soft HEAD^ : 코드는 살리고 commit만 취소하기
- git reset —  merge : merge 취소하기
- git reset — hard HEAD && git pull : git 코드 강제로 모두 받아오기
- git config — global user.name “user_name ” : git 계정Name 변경하기
- git config — global user.email “user_email” : git 계정Mail변경하기
- git stash / git stash save “description” : 작업코드 임시저장하고 브랜치 바꾸기
- git stash pop : 마지막으로 임시저장한 작업코드 가져오기

<br>

### Branch

- git checout : 현재 브랜치 확인
- git checkout "branch_name" : 브랜치 선택
- git checkout -t "remote_path/branch_name" : 원격 브랜치 선택하기
- git branch "branch_name ": 브랜치 생성
- git branch -r : 원격 브랜치 목록보기
- git branch -a : 로컬 브랜치 목록보기
- git branch -m "branch_name" "change_branch_name" : 브랜치 이름 바꾸기
- git branch -d "branch_name ": 브랜치 삭제하기

<br>

<br>

------

**Reference**

- [What is Git](https://www.atlassian.com/git/tutorials/what-is-git)
- [자주 사용하는 기초 Git 명령어 정리하기](https://pks2974.medium.com/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B8%B0%EC%B4%88-git-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-533b3689db81)

