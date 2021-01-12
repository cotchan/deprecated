---
title: Git) Git Branch 개념/ Branch 기본 명령어
author: cotchan
date: 2020-12-07 20:15:21 +0800
categories: [Git, Git_Branch]
tags: [git] 
---


아래 출처를 바탕으로 본인이 이해하기 위해 작성한 포스팅입니다. :)    

브랜치는 한국말로 가지를 의미합니다. git에서는 하나의 근본에서 여러 갈래로 쪼개어 관리할 수 있습니다.    

## Branch 특징

기본은 master 브랜치이고, 이는 필수로 제공되는 브랜치 입니다.    


---


## Branch 중요 개념

1.master를 기준으로, `new를 브랜치(가지치기)하면` master와 똑같은 소스코드가 new에도 적용됩니다.    

2.하지만 이 이후로 new에서 코드를 수정하면 master와 new는 서로 다른 코드가 되기 때문에 갈라집니다.    


---


## Branch 생성 방법

브랜치를 새로 만드신다면 아래의 명령어로 생성합니다.    

```bash
$ git branch [브랜치명]    

//example
$ git branch new
```

 
---


## 생성한 new Branch로 이동
 
+ 생성된 new 브랜치로 접속하기 위한 명령어    

```bash
$ git checkout [브랜치명]    
```

---


## Branch 생성 + 그 Branch로 이동

+ 생성과 동시 브랜치로 이동하고자 한다면 git checkout에 `-b 옵션`을 이용합니다.    

```bash
//example
$ git checkout -b new
```

이 시점에, 생성한 브랜치는 현재 로컬에 저장되어 있습니다.    

협업 작업에서는 생성한 브랜치를 원격 저장소에 등록해줘야 합니다.    


---

## Branch 원격 저장소에 등록

이때는 `git push orgin [브랜치명]`을 이용합니다.    

```bash
//example
$ git push origin new
```


---


## Branch 삭제

+ **로컬 브랜치 삭제**
  + `git branch -d 브랜치명`

+ **원격 브랜치 삭제**
  + `git push origin --delete 브랜치명`  

+ 브랜치 삭제 명령어는 git branch 명령에서 `-d 옵션`을 사용합니다.    
+ 삭제된 브랜치 또한 원격 저장소에 반영을 해야 합니다. (이 때, 브랜치명 앞에 : (콜론)을 붙여줘야 하니 이 점을 유의해야 합니다.)

```bash
//example
$ git checkout master
$ git branch -D new_branch
$ git push origin :new_branch
```


---


## 현재 생성되어 있는 Branch 확인

```bash
//현재 생성되어 있는 브랜치 확인
$ git branch -a
```


---


## Branch 이름 변경

```bash
//브랜치 이름 변경
$ git branch -m ${OLD_BRANCH_NAME} ${NEW_BRANCH_NAME}
```



---
+ 출처
  + [어떻게 깃을 사용하는지 빠르게 알아봅시다](https://github.com/KennethanCeyer/tutorial-git)


