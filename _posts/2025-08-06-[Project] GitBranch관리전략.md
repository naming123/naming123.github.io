---
layout: post
title: Git branch
subtitle: 협업 시 branch 관리
categories: pull request
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

원격레포의 해당 브랜치의 작업과정을 가져옴 (**fetch + merge**를 한 번에 수행)

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


### 4. git pull/push [remote] [branch] 

**[상태확인]**

  git remote -v # 현재 로컬의 [remote]집합 확인
  git branch  # 현재 로컬의 [branch]집합 확인


0. 배경지식
staging area와 같은 부분에 대해서 어떻게 가는지 이해해야됨

1. git add .
add를 하면 해당것들이 blob단위로 만들어짐

2. git commit -m "XXX 기능 구현 / 핫픽스 수정"
**Index 스냅샷을 영구 기록(커밋 객체)**으로 만듦
“스테이징으로 보낸다”가 아니라 스테이징된 것을 커밋으로 박제하는 단계
(선택) 태그: 특정 커밋에 버전 달기


### 만약 최신상태의 다른(브랜치의) 작업을 가져와야할 때
(상황 설정을 한번 하고가자)

1. git status vs git remote -v

2. git stash             # 현재 작업 임시저장
(이건 로컬에서만 저장되는 건가?)

3. git fetch origin      # 원격 저장소 최신 정보 가져오기 (코드 병합 X)
이거 origin 생략해도 되는건가? 이건 remote로 움직이는 건가? 아니면 branch로 움직이는건가?

4. git checkout (another branch)     # 다른 브랜치로 전환

- git switch develop                 # 이건 왜 쓰는 건지 잘 모르겠음
  동일: git checkout develop
  switch는 브랜치 전용, checkout은 브랜치/파일 둘 다 다룸


5. git pull origin (another branch)  # 최신 반영

<-- 다른거 코드 작업 -->

6. git push origin (another branch)  # 사용

7. git stash pop         # 다시 꺼내서 계속 작업


## merge하는 법

1) 로컬에서 바로 머지

git switch develop
git merge feature/A       # fast-forward or merge commit 생성
git push origin develop

2) PR(Pull Request)로 코드리뷰/CI 거쳐 병합(권장)

1. git push origin feature/A
2. GitHub에서 PR 생성 (base: develop, compare: feature/A)
3. 리뷰/승인 → CI 통과 → Merge 버튼

| 메인 배포 시나리오:
| develop 안정화 → develop → main PR → 태그 달고 배포

### 다음 CI/CD 배포와 연결

CI/CD와 연결
PR 생성/업데이트 시 자동으로 테스트/린트/빌드
main/release/*에 머지되면 자동 배포 트리거

예:
on: pull_request → CI
on: push to main → CD(배포)




==========================

원인: 원격(origin)에 feature/nnunetv2 브랜치가 없음(이름 다름/다른 원격에 있음/아직 안 올라감).

바로 확인 → 해결 순서(그대로 복붙):

원격에 브랜치 있는지 확인


git remote -v
git fetch --all --prune
git ls-remote --heads origin | grep -i nnunet
아무 것도 안 나오면 origin엔 없음.

원격 브랜치 목록에서 직접 찾기


git branch -r | grep -i feature
# 예: origin/feature/nnUNetv2  처럼 대소문자/철자 확인
찾은 이름으로 체크아웃


git checkout -t origin/feature/nnUNetv2   # 실제 나온 이름으로
만약 다른 원격(예: intern)에 있다면


git fetch intern
git branch -r | grep intern/feature
git checkout -t intern/feature/nnunetv2
로컬에만 있고 원격엔 아직 없는 경우(네가 만든 로컬 브랜치라면)


git checkout feature/nnunetv2
git push -u origin feature/nnunetv2
그래도 못 찾으면 철자부터 재확인:

feature/nnunetv2 vs feature/nnUNetv2 (대소문자)

슬래시(/) 빠짐 여부 (feature\nnunetv2처럼 개행X)

필요하면 git remote -v 출력 붙여줘. 정확한 원격 이름 기준으로 바로 명령어 써줄게


---

참고자료
- [Git구조](https://velog.io/@bbbb_0221/Git-git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%A1%B0-%EA%B0%84%EB%8B%A8-%EC%82%AC%EC%9A%A9%EB%B2%95)

- [Git명령어조합](https://velog.io/@couchcoding/Git-%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%AA%85%EB%A0%B9%EC%96%B4%EB%93%A4%EC%9D%84-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-1)