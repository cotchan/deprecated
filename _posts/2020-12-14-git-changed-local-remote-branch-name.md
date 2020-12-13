---
title: Git) Local Branch & Remote Branch 이름 변경 방법
author: cotchan
date: 2020-12-14 00:37:21 +0800 
categories: [Git, Git_Branch]
tags: [git]
---

아래 출처를 참고하여 본인 학습 목적으로 작성한 글입니다.    
더 자세한 설명은 출처 포스팅을 참고하시면 됩니다.    

---

## 1. Local Branch 이름 변경 방법

1. 이름을 변경하고자 하는 브랜치로 이동합니다.

```terminal
$ git checkout {OLD_BRANCH_NAME}
```

---

2. 원하는 브랜치 명으로 이름을 변경해줍니다.   

```terminal
$ git branch -m {NEW_BRANCH_NAME}
```

![Desktop View](/assets/img/post/git/2020-12-13-git-changed-local_remote_branch_1.png)

---

+ 아래와 같이 한 번에 바꿀 수도 있습니다.

```terminal
$ git branch -m {OLD_BRANCH_NAME} {NEW_BRANCH_NAME}
```

**위 과정을 거치면 기존에 있던 브랜치가 새로 지정한 브랜치 이름으로 변경됩니다.**


---


## 2. Remote Branch 이름 변경 방법

**Github, Gitlab 등 원격 저장소에 이미 생성되어 있는 브랜치명을 변경하는 법은 위와 다릅니다.**

1. 새로 등록하고자 하는 branch로 checkout 한 후에 `새로운 이름의 브랜치로 push` 합니다.

```terminal
$ git push origin -u {NEW_BRANCH_NAME}
```

![Desktop View](/assets/img/post/git/2020-12-13-git-changed-local_remote_branch_2.png)

---

2. 이전 브랜치명 삭제

위 명령을 실행한 후에 원격 저장소를 확인해보면 기존에 있던 저장소의 이름이 바뀐 것이 아니라 새로운 브랜치가 하나 늘어난 것을 확인할 수 있습니다. 그러므로 이제는 필요없는 예전 이름의 브랜치를 `--delete 옵션을 통해 삭제`해줍니다.

```terminal
$ git push origin --delete {OLD_BRANCH_NAME}
```

![Desktop View](/assets/img/post/git/2020-12-13-git-changed-local_remote_branch_3.png) 



---

+ 출처
	+ [[Git] Local branch, Remote branch 이름 바꾸기](https://readystory.tistory.com/175)
