---
layout: post
title: Git 작업영역
subtitle: Git 명령어를 사용하면 일어나는 일들
categories: git
tags: [git]
---

![banner](/assets/images/0812/banner.png)

Git 명령어를 이해할 때,
"어떤 영역에 있는 파일"의 어떤 부분을, "어떤 영역으로" 
"이동(원본이 없음), 복사(원본이 남음), 삭제(주의 필요)" 중 어느 작업을 하는지 이해하고 사용할 필요가 있다.
(+ 위 명령어를 취소하는 법과 같이 이해하며 넘어가자)

![git 작업영역](/assets/images/0812/0812-1.png)

위는 가장 기본적인 git작업에 대한 여정을 나타낸것이다.

1. 깃 푸쉬 (Local -> Remote)

    git add .
    git commit -m "my name is Peter"
    git push orign main

2. 깃 풀 (Remote -> Local)

    git fetch
    원격에서 최신 이력을 가져오되, 내 코드에는 손대지 않는다.
    (브랜치에 대한 최신이력 업데이트)
    git merge
    그 브랜치의 이력들을 내 코드에 업데이트한다.
    git pull = git fetch + git merge


각각 위 설명에서 "어떤 영역"을 중심으로 나타냈으며
Working Directory, Staging Area, Repository(Local, Remote)을 의미한다.

### Git의 작업영역
1. Working Directory – "실제 개발 작업을 하는" 로컬 파일 공간
2. Staging Area – 커밋할 변경 사항을 잠시 올려두는 준비 영역
3. Local Repository – 로컬에 저장된 "커밋 내역이 있는 저장소(.git 폴더)"
4. Remote Repository – GitHub/GitLab 등 원격 서버의 저장소


## Working Directory
Working Directory는 우리가 실제로 코드 작업을 하는 공간이다.
VSCode, IntelliJ 같은 IDE에서 열어놓고 수정하는 파일들이 이 영역에 해당한다.
이곳이 무너지면(손상되면) 아직 커밋하지 않은 작업 내용이 날아가므로, 어떤 명령어를 사용할 때 버전관리 백업(git stash)도 중요하다.

## Staging Area vs Local Repository

### Staging Area
Staging Area는 .git/index 파일에 저장되는 커밋 전 대기 영역이다.
"git add" 명령을 실행하면 Git은 파일 내용을 blob 객체로 변환하고, 해당 blob 해시와 파일 경로를 index에 기록한다.
즉, 다음 커밋에 포함할 변경 사항 목록을 관리하는 곳이다.

### Local Repository
Local Repository는 .git/objects와 .git/refs 등에 저장된 Git의 핵심 데이터 저장소다.
"git commit" 시, Staging Area의 내용을 기반으로 아까 blob객체를 저장하던 objects폴더에 tree 객체와 commit 객체를 생성하고, 현재 브랜치 포인터(refs/heads/브랜치명)를 최신 커밋으로 업데이트한다.
이곳에는 모든 버전의 코드와 커밋 메타데이터가 보관된다.

여기서 중요한 점은, Working Directory나 Staging Area가 아니라 Local Repository에 만들어진 커밋 객체만이 push할 수 있는 객체라는 것이다.
즉, commit을 하지 않으면 push할 대상 자체가 없기 때문에 원격 저장소로 전송이 불가능하다.

## Remote Repository
Remote Repository는 GitHub, GitLab과 같은 원격 서버에 위치한 저장소다.
.git/config 파일(초기에 세팅한다.)에 원격 저장소의 URL과 이름(origin 등)이 저장되어 있으며,
위에 나타난 git fetch, git pull, git push 명령을 통해 Local Repository와 동기화한다.
원격 브랜치의 정보는 .git/refs/remotes/에 기록된다.


참고링크
https://bill1224.tistory.com/373
https://seongwoojo.github.io/tech-review/Communicate/Git/git%EC%9D%98%20%EC%A0%80%EC%9E%A5%EB%B0%A9%EC%8B%9D.html
https://www.youtube.com/watch?v=xn-kNB_a8CQ&t=1488s
