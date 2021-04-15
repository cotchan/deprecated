---
title: Git) Remote Branch 로컬에 가져오기
author: cotchan
date: 2021-04-15 15:20:21 +0800 
categories: [Git, Git_Basic]
tags: [git]
---

+ 이 글은 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트 될 예정입니다.

---

## 1. 작성 이유

+ **Git을 사용하다보면 `원격 저장소에 있는 branch를 로컬 저장소로 가져와야하는 경우`가 있습니다.** 

---

## 2. 원격 저장소의 branch 가져오기

---

## 2-1. git remote update

+ 먼저 원격의 브랜치에 접근하기 위해 git remote를 갱신해줄 필요가 있습니다.

```bash
$ git remote update
```

---

## 2-2. git checkout -t 

+ 명령어
  + **`$ git checkout -t {REMOTE_BRANCH_NAME}`**

---

![Desktop View](/assets/img/post/git/2021-04-15-git-get-remote-branch.png)

+ 만약 위의 상황에서 원격 저장소의 `feature/create-meeting` branch를 가져오고 싶다면 **아래 명령어를 입력하면 됩니다.**

```
$ git checkout -t origin/feature/create-meeting
```

+ **`-t 옵션`과 원격 저장소의 branch 이름을 입력하면** 
  + 로컬에 원격 브랜치와 동일한 이름의 branch를 생성하면서 해당 branch로 checkout을 합니다.
+ 만약 branch 이름을 변경하여 가져오고 싶다면
  + `$ git checkout -b [생성할 branch 이름] [원격 저장소의 branch 이름]` 처럼 사용하면 됩니다.


---

+ 출처
    + 정호영, 진유림, 『팀 개발을 위한 Git-GitHub 시작하기』, 한빛미디어(2020), p94
    + [Git remote branch 가져오기](https://cjh5414.github.io/get-git-remote-branch/)
