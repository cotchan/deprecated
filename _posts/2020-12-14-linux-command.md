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

## 2. rm (파일, 디렉토리 삭제)

## 2-1. 파일 삭제 ($ rm FILE_NAME)

```terminal
$ rm FILE_NAME
```

+ **rm file1.txt**
	+ file1.txt 파일을 삭제한다.
+ **rm `*.dat`**
	+ '.dat'로 끝나는 파일을 모두 삭제한다.
+ **rm ***
	+ 모든 파일을 삭제한다.

---

## 2-2. 디렉토리 삭제 ($ rm -r DIR_NAME)

```terminal
$ rm -r DIR_NAME
```

+ 디렉토리를 삭제하기 위해서는 `-r 옵션`을 사용해야 합니다. (recursive)
    + rmdir 명령과는 달리 파일이 들어있는 디렉토리도 삭제합니다.

1. **rm `-r` dir1/**
2. **rm `-rf` dir1/**
    + r 옵션과 함께 f 옵션을 사용하게 되면 경고 없이 모두 강제로 삭제합니다.
3. **rm `-ri` dir1/**
    + 디렉토리에 있는 내용을 하나 하나 확인하면서 삭제하기 위해서는 `i 옵션`을 사용합니다.


---

## 3. mv (이동, 이름 변경)

리눅스에서는 mv 명령을 이용하여 `파일 이동(move)`을 할 수 있습니다. 같은 폴더에서 파일, 디렉토리 이동을 하는 경우 `이름변경 효과`가 있습니다.

---

## 3-1. mv file1 file2 (파일명 변경)

```terminal
$ mv file1 file2
```

+ file1 파일을 file2 파일로 `이름을 변경`합니다.

---

## 3-2. mv file1 dir1/ (파일 이동)

+ `앞에 파일이름`이 오고, `뒤에 디렉토리`가 오는 경우

```terminal
$ mv file1 dir1/
$ mv file1 file2 dir1/ 
```

+ file1 파일을 dir1 디렉토리로 이동합니다.
+ 두 번째 경우처럼 여러 개의 파일을 한번에 이동시킬 수 있습니다.


---

## 3-3. mv dir1/ dir2 (디렉토리명 변경)

```terminal
$ mv dir1/ dir2/
```

+ dir1 디렉토리를 dir2 디렉토리로 `이름을 변경`합니다.


---

## 4. ls (디렉토리 내용 출력)

리눅스에서는 ls 명령어를 사용하여 디렉토리에 있는 내용(디렉토리, 파일 등)을 확인합니다. 윈도우의 dir 명령과 비슷합니다.

---

## 4-1. 자주사용하는 ls 명령어 옵션

+ **`-a` (all)**
+ **`-l` (long)**
+ **`-s` (size)**
+ **`-r` (recursive)**


---

## 5. curl

향후 업데이트 예정입니다. :)

+ 다양한 프로토콜을 지원하는 데이터 전송용 Command Line Tool 입니다.
+ HTTP, HTTPS, FTP, SFTP, SMTP 등을 지원합니다.


---
+ 출처
	+ [알아야 할 리눅스 파일 및 디렉터리 기본 명령어 1탄 및 관련문제(cd,ls,pwd,touch 등)](https://jhnyang.tistory.com/13?category=815412)
	+ [파일을 생성하는 명령어 - touch, cat, vi](https://kwmblog.tistory.com/8)        
	+ [리눅스 rm 명령어, 옵션 사용법 정리 (파일, 디렉토리 삭제 명령)](https://withcoding.com/95)
	+ [cURL 소개, HTTP GET, POST 호출 방법](https://blog.naver.com/PostView.nhn?blogId=wideeyed&logNo=221350638501&proxyReferer=https:%2F%2Fwww.google.com%2F)
	+ [curl GET/POST/DELETE 전송](https://developyo.tistory.com/11)
