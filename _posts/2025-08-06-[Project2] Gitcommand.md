---
layout: post
title: Git Command
subtitle: Git 명령어 사전
categories: Project
tags: [git]
---

![banner](/assets/images/0806/0806-1.png)

Git을 시작해보자
전제: 
1. github에서 협업할 레포링크를 받았다 (ex. https://github.com/naming123/JAVA_baekjoon.git)
2. git bash 등 git과 관련된 프로그램을 모두 다운받았다.

#####  모든 명령어는 현상황 상태 확인 -> 명령어를 위한 상태설정 -> 명령어 실행  (-> 이전 상황 복원) 으로 이해한다. 
각 번호의 명령어에서 []에 있는 것을 확인하면 된다.

### 1. git init
현재 폴더에 .git/을 만들고 로컬 Git 저장소로 초기화한다.
커밋, 브랜치, 리모트 정보 등은 모두 .git/ 내부 메타데이터로 관리된다.
=> 작업할 공간에서 설정해야함 (이를 기준으로 repo와 연결됨)

**[상태확인]**

  git status # 현재 로컬의 워킹디렉토리(staging area 포함) 경로 확인

<!-- - 로컬 저장소가 어떻게 움직이고 GitHub과 어떻게 연결됨?
로컬: 워킹디렉토리(파일) ↔ Index(Staging Area) ↔ 로컬 커밋 히스토리

원격(GitHub): origin 같은 리모트를 추가해 푸시/풀로 동기화

한번더 한다고해서 어떤 변화가 생기는가?
- 기존 기록/설정 안 날아감. (실수로 상위 폴더에서 git init 금지) -->

**[복원(제거)방법]**

  rm -rf .git  # Linux/Mac
  rmdir /s .git  # Windows
  => 그냥 .git폴더를 지우면 됨


### 2. git remote add [remote] [repo주소]
(origin https://github.com/naming123/JAVA_baekjoon.git)

github에서 연결관리는 remote를 통해 연결시킨다.
=> 내가 원하는 레포로 연결을 시켜줌

**[상태확인]**

  git remote -v # 현재 로컬의 [remote]집합 확인
  git remote add origin (https://github.com/naming123/JAVA_baekjoon.git) # [remote]로 [repo주소]와 연결

현재 로컬 저장소에 원격 별칭 origin 추가
이후 git fetch/pull/push origin 등 로컬과 원격 사이의 연결관계를 설정

아래는 잘 사용하는 remote 이름을 정리해둔것이다. (말그대로 이름일뿐이다.)

| 리모트 이름 | 용도 |
|-------------|------|
| origin      | 기본 작업 대상(내 포크 or 중앙 저장소) |
| upstream    | 원본 프로젝트(포크했을 때) |
| prod        | 배포용 저장소 |
| backup      | 별도 백업 저장소 |
| father      | 그냥 원본을 이렇게 부르기로 한 경우 |

**[복원(제거)방법]**

  git remote rm [remote]   # origin의 remote 제거 


### 3. git checkout/branch [branch] 
(origin main)

내가 사용할 혹은 접근할 branch를 생성/이동(HEAD로 접근)/삭제한다.

**[상태확인]**

  git fetch [remote] [branch] # 로컬의 상태정보를 원격의 상태로 최신화
  git branch               # 현재 브랜치 확인 (default는 로컬 레포의 브랜치고, -r, -a로 원격도 확인할 수 있음)
  =>  git branch -r 인 경우는 원격브랜치(마지막 pull/fetch시점), git branch는 내 컴퓨터에서 만든 branch를 보여줌

그 뒤 git checkout을 통해 해당 branch로 이동(HEAD 설정)하는 것이다.

##### remote vs  branch

remote는 원격 레포에, branch는 그 레포안에서 관리되는 workflow들이라고 생각하면 된다. 

  1. remote를 통해 연결된 레포를 확인하고 
  2. 그 안의 최신정보는 fetch로 (git fetch가 remote에 연결된 레포로 접속함)
    => git fetch (remote ex. origin)으로 따로 연결가능
  3. 그 뒤 내가 들어갈 곳을 branch로 접속하면 된다.

##### git checkout vs git branch

HEAD로 인식하는가? vs 포인터 설정만 하는가?

  git branch feature/XXX # feature/XXX 브랜치를 생성하고 포인터 연결까지만 함
  git checkout -b feature/XXX # feature/XXX 브랜치를 생성하고 그걸 HEAD로 인식 후 이동

branch 관리에 대한 규칙도 있다.
=> 다음에 협업플젝관련으로 posting하겠다.

cf) feature/XXX 브랜치를 새로 만들고 전환
=> 요즘 문법: git switch -c feature/XXX (안전하게 HEAD로 이동)
(하지만 왜 쓰는지는 아직은 잘 모르겠다.)

**[복원방법]**

  git branch -d [branch]  # 로컬 브랜치 삭제
  git push origin --delete [branch]  # 원격 브랜치 삭제
  (checkout으로는 삭제 불가능)

### 4. git pull/push [remote] [branch] 
원격레포의 해당 브랜치의 작업과정을 가져옴 (**fetch + merge**를 한 번에 수행)

**[상태확인]**

  git remote -v # 현재 로컬의 [remote]집합 확인
  git branch  # 현재 로컬의 [branch]집합 확인


1. git add .
add를 하면 해당것들이 blob단위로 만들어짐

2. git commit -m "XXX 기능 구현 / 핫픽스 수정"
**Index 스냅샷을 영구 기록(커밋 객체)**으로 만듦
“스테이징으로 보낸다”가 아니라 스테이징된 것을 커밋으로 박제하는 단계
(선택) 태그: 특정 커밋에 버전 달기


**[복원방법]**

  git reflog               # log폴더의 텍스트로그를 따라 복원
  git reset --hard HEAD@{1} # reflog로 이전 상태 복원
  => HEAD를 기준으로 <.git/objects/>에 저장되어 있는 시계열로그
  (완전 로컬전용으로 90일정도의 보존기간이 있다.)

  .# 또는

  git log # 커밋 그래프를 따라 추적
  git reset --hard HEAD~1  # 마지막 커밋 취소
  <.git/objects/>에 저장되어있는 log들을 git graph를 따라가며 탐지
  => git flow에 끊긴 branch는 접근 불가

##### cf) 만약 최신상태의 다른(브랜치의) 작업을 가져와야할 때
(현재 만들고 있는 기능 중에 다른 브랜치의 작업을 봐야하거나(코드리뷰, 참고 등), merge할 때 충돌이 나서 해당 branch를 불러와야할 때)

**[상태확인]**

  git stash list           # 현재 저장된 stash 목록 확인
  git diff                 # 현재 변경사항 확인

1. stach 생성

  git stash             # 현재 작업 임시저장
  git stash                # 현재 변경사항을 stash로 저장
  git stash save "메시지"   # 메시지와 함께 stash 저장
  git stash push -m "메시지" # 최신 문법


2. git pull/push [remote] [branch] 후 작업

3. git stash pop         # 다시 꺼내서 계속 작업


### merge하는 법

1) 로컬에서 바로 머지

  git switch dev
  git merge feature/A       # feature/A를 dev branch에 설정
  git push origin dev

2) PR(Pull Request)로 코드리뷰/CI 거쳐 병합(권장)

  1. git push origin feature/A
  2. GitHub홈페이지에서 PR 생성 (base: develop, compare: feature/A)
  3. 리뷰/승인 → CI 통과 → Merge 버튼

| 메인 배포 시나리오:
| develop 안정화 → develop → main PR → 태그 달고 배포

### 다음 CI/CD 배포와 연결

(optional) 이에 대한 자동화 검정기능 필요 (by devOPs엔지니어 with GithubAction)

예:
on: pull_request → CI
on: push to main → CD(배포)



---

참고자료
- [Git구조](https://velog.io/@bbbb_0221/Git-git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%A1%B0-%EA%B0%84%EB%8B%A8-%EC%82%AC%EC%9A%A9%EB%B2%95)

- [Git명령어조합](https://velog.io/@couchcoding/Git-%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%AA%85%EB%A0%B9%EC%96%B4%EB%93%A4%EC%9D%84-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-1)