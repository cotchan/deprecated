---
title: Git add 취소하는 방법
author: cotchan
date: 2020-12-07 20:00:21 +0800
categories: [Git, Git_ETC]
tags: [git]
---

## ADD 취소하기

+ 파일 상태를 Unstage로 변경하기

실수로 `git add *` 명령을 사용하여 모든 파일을 Staging Area에 넣은 경우,     
`Staging Area`(git add 명령을 수행한 이후의 상태)에 넣은 파일을 빼고 싶을 때 사용합니다.    

```bash
$ git reset HEAD // 전체 add 취소 
$ git reset HEAD [file] //특정 파일에 대한 add 취소
```

git status로 파일들의 상태를 확인 후 다시 add를 하면 됩니다. :)
