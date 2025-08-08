---
layout: post
title: Blog Start
subtitle: 블로그 시작하는법
categories: git
tags: [git]
---

![banner](/assets/images/banners/home.jpeg)

Git을 시작해보자
전제: 
1. github에서 협업할 레포링크를 받았다 (ex. https://github.com/naming123/JAVA_baekjoon.git)
2. git bash 등 git과 관련된 프로그램을 모두 다운받았다.


### 초기세팅

1. git init
현재 폴더에 .git/을 만들고 로컬 Git 저장소로 초기화함.
커밋, 브랜치, 리모트 정보 등은 모두 .git/ 내부 메타데이터로 관리됨.

- 로컬 저장소가 어떻게 움직이고 GitHub과 어떻게 연결됨?
로컬: 워킹디렉토리(파일) ↔ Index(Staging Area) ↔ 로컬 커밋 히스토리

원격(GitHub): origin 같은 리모트를 추가해 푸시/풀로 동기화

한번더 한다고해서 어떤 변화가 생기는가?
- 기존 기록/설정 안 날아감. (실수로 상위 폴더에서 git init 금지)

2. git remote add origin (https://github.com/naming123/JAVA_baekjoon.git)
현재 로컬 저장소에 원격 별칭 origin 추가
이후 git fetch/pull/push origin ... 형태로 원격과 동기화

| 리모트 이름 | 용도 |
|-------------|------|
| origin      | 기본 작업 대상(내 포크 or 중앙 저장소) |
| upstream    | 원본 프로젝트(포크했을 때) |
| prod        | 배포용 저장소 |
| backup      | 별도 백업 저장소 |
| father      | 그냥 원본을 이렇게 부르기로 한 경우 |


3. git pull origin main # 최신 코드 기준 맞춤
**fetch + merge**를 한 번에 수행
아직 로컬에 커밋이 없다면 fast-forward로 원격 main을 그대로 가져와 맞춤

4. git checkout -b feature/XXX  # 기능 브랜치 생성
feature/XXX 브랜치를 새로 만들고 전환
요즘 문법: git switch -c feature/XXX (브랜치 전용 명령, 안전함)


## 작업 및 commit

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











