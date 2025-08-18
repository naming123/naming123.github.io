---
layout: post
title: Blog Start
subtitle: 블로그 시작하는법
categories: git
tags: [git, Blog]
---

![banner](/assets/images/0724/banner.png)

github 블로그를 시작했다. 
특히 프로젝트 단위로 공부한 내용을 정리하고, 이를 다시 참고하거나 공유할 수 있도록 구조화된 공간이 필요했다. 그래서 GitHub 블로그와 네이버 블로그를 병행해서 운영하기로 했다.

내가 이해한 github 블로그와 네이버 블로그의 차이는 다음과 같다.

### Github Blog의 장점
  * git push와 commit을 통한 연결관계 관리
  * Markdown에 대한 연습
  * 블로그 레이아웃이나 구성요소 인터렉션에 대한 자유로운 수정
  * Github repo나 project스케줄링과 연결하여 프로젝트 아카이브처럼 사용가능

### Naver Blog의 장점
  * git이나 markdown을 사용하지 않고 원래 글 쓰는 방식대로 그대로 사용가능
  * 이미지나 링크, 비디오를 넣는 것이 직관적
  * 각 마크다운(콜아웃, "---" 등)에 대한 UI 스타일같은 것들이 매핑 완료되어있음
  * 검색 유입이 용이하고, 깃헙 사용자들과 또 다른 사용자 층을 보유


따라서 다음과 같이 용도를 구분했다.
1. Github Blog: 프로젝트 중심의 History 및 기술 Archive
2. Naver Blog: CS개념 공부, 기술개발외의 새로운 인사이트 및 사실(다시 자세히 정리하고 싶을 때) 등

---
## Github Blog 만드는 법

깃허브 블로그는 다음 링크를 참고했다.
https://seongwoojo.github.io/Tutorial/Github%20tutorial/post-01.html

GitHub Blog 시작법은 간단하게 설명하면 다음과 같다.
1. {계정이름}.github.io으로 repo 만들기.
2. [원하는 템플릿](http://jekyllthemes.org/)을 골라서 해당 깃repo를 clone하기
3. git clone한 해당 폴더에서 markdown을 통해 ".md" 파일을 작성하기
4. Commit & Push하고 github action을 통해 배포하기

내가 고른 템플릿은 다음과 같다.
![템플릿사진](http://jekyllthemes.org/thumbnails/jekyll-theme-yat.png)
http://jekyllthemes.org/themes/jekyll-theme-yat/



### 설치방법

자세한 내용은 다음을 참고하자 
https://jekyllrb.com/docs/themes/#understanding-gem-based-themes

구성요소는 다음과 같다.
![구성요소](/assets/images/0724/image.png)

많은 요소들이 있지만 우리는 여기서 "_post"에 있는 "예시.md" v파일에 접근하면 되고 
파일 제목도 유의하여 "날짜-제목.md"로 작성해야된다.

![적용모습](project1/assets/images/0724/blogtitleMeaning.png)


assets 폴더는 우리가 올릴 이미지와 비디오, 스타일 등을 결정하고 업로드 할 수 있는 공간이다.
원래는 각 웹 링크로 들어가 이미지 주소 복사 등을 통해 접근해도 되지만 
그 파일에 대한 권한이 우리에게 없기 때문에 임의로 삭제가 되거나 찾을 수 없게 되기 때문에 저장하여 assets폴더로 접근하는 것이 더 안전하다.

---

참고할 마크다운도 정리해보자.


### 1. 텍스트 스타일링

| 기능 | 문법 | 결과 |
|------|-------|--------|
| 굵게 | `**bold**` 또는 `__bold__` | **bold** |
| 기울임 | `*italic*` 또는 `_italic_` | *italic* |
| 굵은 기울임 | `***bold italic***` | ***bold italic*** |
| 취소선 | `~~취소~~` | ~~취소~~ |
| 인라인 코드 | `` `코드` `` | `코드` |
| 이스케이프 | `\*text\*` | \*text\* |
| 큰따옴표 | `"큰따옴표"` | "큰따옴표" |
| 작은따옴표 | `'작은따옴표'` | '작은따옴표' |
| 콜아웃 | `>콜아웃` | >콜아웃 |


### 2. 제목과 구분선

| 기능 | 문법 | 결과 |
|------|-------|--------|
| 제목 1~6단계 | `#` ~ `######` | 제목 크기 |
| 수평선 | `---`, `***`, `___` | ──────── |



### 3. 목록 (List)

| 기능 | 문법 | 결과 |
|------|-------|--------|
| 순서 없는 목록 | `- 항목`, `* 항목`, `+ 항목` | • 항목 |
| 순서 있는 목록 | `1. 항목` | 1. 항목 |
| 중첩 목록 | 들여쓰기 2~4칸 + `-`, `*`, `1.` | 계층적 목록 |
| 정의 목록 | `단어`<br>`: 정의 내용` | 정의 리스트 |



### 4. 링크 및 이미지

| 기능 | 문법 | 결과 |
|------|-------|--------|
| 링크 | `[텍스트](https://example.com)` | [텍스트](https://example.com) |
| 내부 링크 | `[이동](#section-title)` | 내부 섹션 이동 |
| 이미지 | `![alt](img_url "설명")` | 이미지 표시 |
| 로컬 문서 링크 | `[문서](./README.md)` | 내부 파일 링크 |





