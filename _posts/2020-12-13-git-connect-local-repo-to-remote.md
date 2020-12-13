---
title: 로컬 폴더를 원격 저장소와 연결하기 
author: cotchan
date: 2020-12-13 19:00:21 +0800 
categories: [Git, Git_ETC]
tags: [git]
---

아래 출처를 참고하여 작성한 글입니다.    

---

## 1. 원격 저장소에 연결할 로컬 폴더에서 git init

![Desktop View](/assets/img/post/git/2020-12-13-git-connect-local-repo.png)

---

## 2. Github에 원격 저장소(Repository) 만들기

---


## 3. 다시 로컬 폴더에서 연결 명령어 입력하기

다시 로컬 폴더에서 터미널을 열고 `git remote add origin {깃허브 리포지터리 URL}`을 입력합니다.        

```terminal
$ git remote add origin https://github.com/cotchan/{REPO_NAME}.git
```



---

+ 출처
	+ [[Git] 로컬 폴더를 원격 저장소와 연결하기](https://webstudynote.tistory.com/90)
