---
title: Linux) 자주사용하는 리눅스 명령어 모음
author: cotchan 
date: 2020-12-14 18:54:21 +0800 
categories: [Linux, Linux_Command]
tags: [linux]
---


본인 참고 용도로 작성한 포스팅입니다.    
계속 업데이트 될 예정이며 아래 출처의 내용들을 바탕으로 작성했습니다.        


---


## 1. touch (파일 생성)

+ 파일의 타임스탬프를 변경하거나 파일의 크기가 0인 `빈 파일을 생성하는 명령어`

```terminal
$ touch [option] 파일명
$ touch a.txt
```

+ 위 명령어를 입력하면
    1. a.txt라는 파일이 존재하면 `파일의 수정 시간을 바꿉니다.`
    2. 파일이 없는 경우에는 a.txt라는 파일 크기가 0인` 빈 파일을 생성합니다.` 

![Desktop View](/assets/img/post/linux/2020-12-14-linux-touch.png)

---

## 1-1. touch 와 vi의 차이점

+ touch
    + touch 명령어를 사용하면 `아무 내용이 없는 파일이 생성`됩니다.
+ vi
    + vi 명령어는 `문서 편집 명령어`입니다. 
    + vi 파일명을 입력하면 임시 파일이 생성되는데, :w를 누르지 않고 :q만 누르면 파일은 생성되지 않고 사라집니다.



---
+ 출처
	+ [알아야 할 리눅스 파일 및 디렉터리 기본 명령어 1탄 및 관련문제(cd,ls,pwd,touch 등)](https://jhnyang.tistory.com/13?category=815412)
	+ [파일을 생성하는 명령어 - touch, cat, vi](https://kwmblog.tistory.com/8)
