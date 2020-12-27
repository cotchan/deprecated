---
title: Swift) Bridge Header 적용방법
author: cotchan
date: 2020-12-24 10:00:21 +0800
categories: [Swift, Swift_Basic]
tags: [swift] 
---

이 글은 아래 출처를 바탕으로 작성한 내용입니다.

---

## 1. Bridge Header 사용이유

+ **`swift`에서 `Objective-C 코드를 사용`하기 위해 브릿지 헤더를 사용합니다.**

---

## 2. 브릿지 헤더 만드는 방법

## 2-1. 브릿징 헤더 만들기

+ 프로젝트 우클릭 -> New File -> Header File -> Next

---

## 2-2. 프로젝트명-Bridging-Header.h로 파일저장

+ 만들 헤더 파일명은 `프로젝트명-Bridging-Header.h` 입니다.
+ **Naming convention은 `필수사항은 아닙니다.`**

---

## 2-3. 프로젝트 클릭 -> Build Settings

1. 프로젝트 선택 -> Build Settings -> Swift Compiler 검색
2. Objective-C Bridging Header 더블클릭해서 프로젝트명-Bridging-Header.h 타이핑 후 엔터를 칩니다.

![Desktop View](/assets/img/post/swift/2020-12-24-swift-bridge-header.png)

---

## 2-4. 생성한 브릿지헤더에 포함하고자 하는 Objective-C 헤더파일 추가

```ruby
# in {MY_PROJECT_NAME}_Bridging_h

#ifndef {MY_PROJECT_NAME}_Bridging_h
#define {MY_PROJECT_NAME}_Bridging_h

#import "objective-c-header1.h"
#import "objective-c-header2.h"
#import "objective-c-header3.h"
#import "objective-c-header4.h"

#endif /* {MY_PROJECT_NAME}_Bridging_h */
```

+ 여기까지 완료하셨다면 이제 스위프트 파일에서 원하는 Objective-C 소스코드를 사용할 수 있습니다.

---


## 3. 추가로 필요한 사항

+ 브릿지 헤더가 참조할 수 있는 위치에 header 파일 생성
    1. **이 `header file`은 `인터페이스`에 해당하는 역할입니다.**
    2. 이 header file에 `실제 참조하고자하는` objective-c header file 경로를 입력합니다.

```ruby
# objective-c-header1.h

#include "../../directory/directory/project_name/objective-c-header1.h"
```


---

+ 출처
    + [애플 스위프트(Apple Swift) Bridging-Header.h 생성하기](https://m.blog.naver.com/seotaji/220312825002)
