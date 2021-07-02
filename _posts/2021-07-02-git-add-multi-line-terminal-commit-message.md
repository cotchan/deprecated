---
title: Git) 터미널에서 여러 줄의 커밋(commit) 메시지 추가하기
author: cotchan
date: 2021-07-02 17:00:21 +0800 
categories: [Git, Git_Basic]
tags: [git]
---

+ 이 글은 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트 될 예정입니다.

---

## 1. Git commit -m 개행

+ **여러 줄의 커밋문을 작성할 때에, 개행은 아래와 같은 방법으로 할 수 있습니다.**

```terminal
$ git commit -m "1줄
> 2줄
> 3줄
> 4줄
"
```

+ **`"`로 시작한 커밋문은 그다음 `"`를 만나기 전까지는 엔터를 눌러도 명령어로 인식하지 않기 때문에, 그냥 엔터를 눌러주면 개행이 가능합니다.**

---

+ 출처
  + [[Git] 여러 줄의 커밋(commit) 메시지 추가하기](https://enfanthoon.tistory.com/98)
