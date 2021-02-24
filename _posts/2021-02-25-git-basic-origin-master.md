---
title: Git) origin, master
author: cotchan
date: 2021-02-25 08:46:21 +0800 
categories: [Git, Git_Basic]
tags: [git]
---

+ 이 글은 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트 될 예정입니다.

---

## 1. origin이란

+ **origin은 우리가 연결한 `GitHub 원격저장소의 닉네임`입니다.**
+ 그래서 아래 명령어는 `"origin이란 이름으로 원격저장소를 추가하라"는 뜻`입니다.

```terminal
$ git remote add origin https://github.com/cotchan/git-practice.git 
```

---

## 2. master란

+ master란 우리가 커밋을 올리는 줄기(branch)의 이름입니다.
+ **사용자가 따로 줄기(branch)를 생성하지 않으면 Git은 `master라는 기본 branch`에 커밋을 올립니다.**


---

## 3. [master] / [origin/master]

+ 그러므로 [master]는 내 컴퓨터 `로컬저장소의 버전`을 의미합니다.
+ 그러므로 [origin/master]는 `GitHub 원격저장소의 버전`을 가리킵니다.


---

## 4. git push origin master

+ **이 명령은 "현재 로컬 저장소의 줄기(branch)인 `master의 모든 새로운 커밋을 원격 저장소에 올리겠다`"는 뜻입니다.**
+ 이 명령이 수행되면 로컬저장소의 branch와 원격 저장소 branch가 모두 같은 최신의 커밋을 가리키게 됩니다.



---

+ 출처
    + 정호영, 진유림, 『팀 개발을 위한 Git-GitHub 시작하기』, 한빛미디어(2020), p71-p73
