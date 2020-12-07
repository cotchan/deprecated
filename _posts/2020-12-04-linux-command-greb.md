---
title: 리눅스 grep 명령어 사용 방법
author: cotchan
date: 2020-12-04 12:48:50 +0800
categories: [Linux, Command]
tags: [linux]     # TAG names should always be lowercase
---


**<목차>**

- grep 이란
- grep 명령어 옵션
- grep 사용 예시
    - grep 사용 예시 - 특정 파일 검색하기

---

### 1. grep 이란

**리눅스 명령어 grep 이란, 입력으로 전달된 파일의 내용에서 특정 문자열을 찾고자 할 때 사용**

- grep을 사용하여 파일로부터 문자열을 검색하는 방법은 아래와 같습니다.

```cpp
$ grep [OPTION] [PATTERN] [FILE]
```

### * grep으로 특정 파일 검색하기

현재 디렉토리의 파일과 폴더를 확인할 때 사용하는 ls 명령어와 특정 패턴의 문자열을 검색하는 grep 명령어를 조합하여 특정 파일을 검색할 수 있습니다.

---

### 2. grep 명령어 옵션

grep 명령에서 사용할 수 있는 옵션은 아래와 같습니다.

더 자세한 사항은 "grep-help" 명령을 통해 확인 가능

```cpp
grep [OPTION...] PATTERN [FILE...]
        -E        : PATTERN을 확장 정규 표현식(Extended RegEx)으로 해석.
        -F        : PATTERN을 정규 표현식(RegEx)이 아닌 일반 문자열로 해석.
        -G        : PATTERN을 기본 정규 표현식(Basic RegEx)으로 해석.
        -P        : PATTERN을 Perl 정규 표현식(Perl RegEx)으로 해석.
        -e        : 매칭을 위한 PATTERN 전달.
        -f        : 파일에 기록된 내용을 PATTERN으로 사용.
        -i        : 대/소문자 무시.
        -v        : 매칭되는 PATTERN이 존재하지 않는 라인 선택.
        -w        : 단어(word) 단위로 매칭.
        -x        : 라인(line) 단위로 매칭.
        -z        : 라인을 newline(\n)이 아닌 NULL(\0)로 구분.
        -m        : 최대 검색 결과 갯수 제한.
        -b        : 패턴이 매치된 각 라인(-o 사용 시 문자열)의 바이트 옵셋 출력.
        -n        : 검색 결과 출력 라인 앞에 라인 번호 출력.
        -H        : 검색 결과 출력 라인 앞에 파일 이름 표시.
        -h        : 검색 결과 출력 시, 파일 이름 무시.
        -o        : 매치되는 문자열만 표시.
        -q        : 검색 결과 출력하지 않음.
        -a        : 바이너리 파일을 텍스트 파일처럼 처리.
        -I        : 바이너리 파일은 검사하지 않음.
        -d        : 디렉토리 처리 방식 지정. (read, recurse, skip)
        -D        : 장치 파일 처리 방식 지정. (read, skip)
        -r        : 하위 디렉토리 탐색.
        -R        : 심볼릭 링크를 따라가며 모든 하위 디렉토리 탐색.
        -L        : PATTERN이 존재하지 않는 파일 이름만 표시.
        -l        : 패턴이 존재하는 파일 이름만 표시.
        -c        : 파일 당 패턴이 일치하는 라인의 갯수 출력.
```

---

### 3. grep 사용 예시 - 특정 파일 검색하기

1. 대상 파일에서 문자열 검색
    - grep "STR" [FILE]
2. 현재 디렉토리 모든 파일에서 문자열 검색
    - grep "STR" *
3. '2017_'로 시작하는 파일 찾기
    - `$ ls -al | grep '2017_'`
4. 확장자가 '.xxx'인 파일 찾기

    ```cpp
    $ ls -al | grep '.*[.]xxx'
    $ ls -al | grep '.*\.xxx'
    ```

5. 확장자가 '.xxx'인 파일 찾기 & 현재 디렉토리의 하위 폴더까지 포함해서 검색

    ```cpp
    $ ls -alR | grep '.*[.]xxx'
    ```

6. 하위 폴더 포함하여 특정 폴더(폴더명: temp) 검색
    - `$ ls -alR | grep 'temp$'`

---

```cpp
//특정 파일 검색 사용 예시
//확장자가 .xib인 파일 찾기 & 현재 디렉토리의 하위 폴더까지 포함해서 검색
$ ls -alR | grep '.*[.]xib'

MainMenu.xib
LaunchScreen.xib
objects.xib
BloggerSampleWindow.xib
BooksSampleWindow.xib
CalendarSampleWindow.xib
EditEventWindow.xib
ContactsSampleWindow.xib
EditEntryWindow.xib
```

---

<출처>

- [https://recipes4dev.tistory.com/157](https://recipes4dev.tistory.com/157)
- http://blog.naver.com/PostView.nhn?blogId=tollu09&logNo=220986391076&categoryNo=37&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView
