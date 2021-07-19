---
title: Git) 이전 commit으로 돌아가기
author: cotchan
date: 2021-07-19 17:48:21 +0800 
categories: [Git, Git_Basic]
tags: [git]
---

+ 이 글은 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트 될 예정입니다.

---

## 1. git log

+ 특별한 아규먼트 없이 `git log` 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여줍니다.

```terminal
$ git log
```

---

## 2. commit 되돌리기

```
//commit sample

commit 9190a3ac
commit 32edd1ed (HEAD)
commit 7f4d2367
commit b61f5f6e
```

---

## 2-1. git reset

+ 그냥 reset을 이용한 경우, `reset으로 돌아온 커밋 이후의 변경사항은 모두 unstaged 영역에 남습니다.`

```terminal
$ git reset {어디로 돌아갈지}
```

---

## 2-2. soft reset

+ 그냥 reset이 변경사항을 unstaged 영역에 남겼다면, `soft reset은 staged 영역에 남깁니다.` 
  + 그래서 `git commit` 을 했을 때 기존 상태로 돌아오게 됩니다.
 

```terminal
$ git reset --soft {어디로 돌아갈지}
```

---

## 2-3. hard reset

+ 타노스 리셋입니다. **돌아가는 `commit 로그보다 앞서는 변경사항을 모두 제거`합니다.**
+ 위 예제에서 hard reset을 사용했다면, 9190a3ac 에서 있었던 변경사항은 로컬에서 모두 사라진다.

```terminal
$ git reset --hard {어디로 돌아갈지}
```

---

+ 출처
  + [[git] 이전 commit으로 돌아가기](https://medium.com/@kwoncharles/git-%EC%9D%B4%EC%A0%84-commit%EC%9C%BC%EB%A1%9C-%EB%8F%8C%EC%95%84%EA%B0%80%EA%B8%B0-cf6caed43ed5)
  + [2.3 Git의 기초 - 커밋 히스토리 조회하기](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0)
