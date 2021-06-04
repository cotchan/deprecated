---
title: ios) 화면 회전 방지
author: cotchan
date: 2021-06-04 09:12:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 전체방향 설정

+ **`Supported interface orientations`**
  + **Supported interface orientations (iPad) `Portrait`(bottom home button)만 선택해서 가로 보기 방지가 가능합니다.**

![스크린샷 2021-06-04 오전 9 13 44](https://user-images.githubusercontent.com/75410527/120727730-44d9aa00-c516-11eb-90f5-1b3230ce8d6c.png)

+ 앱은 기본적으로 `Info.plist`의 `Supported interface orientations`을 참조합니다.
+ 여기에 Info.plist 값을 지우거나 더함으로써 앱의 기본방향을 정해줄 수 있습니다.
  + **`가로 보기 방지`를 원한다면 `Portrait(bottom home button)`만 남겨놓으면 됩니다.**

---

## 2. Requires Full Screen

+ 가로보기 방지를 위해서는 추가적으로 Status Bar Style => `Requires full screen` YES 옵션이 필요합니다.

+ 이 옵션이 필요한 이유는 iPad는 multitasking feature라는 게 있습니다.
+ 그러나 multitasking feature를 사용하는 경우 Supported interface orientations (iPad) 4가지 속성을 전부 지원해야하는 문제가 있습니다.

+ **multitasking feature를 사용하지 않는 App은 `Status Bar Style` => `Requires full screen`을 선택하면 Supported interface orientations (iPad) 속성을 전부 선택하지 않아도 됩니다.**


---

+ 출처
  + [iOS 화면 회전 처리](https://wnstkdyu.github.io/2017/12/15/orientations/)
  + [What is the impact of the “Requires full screen” option in Xcode for an iPhone-only app?](https://stackoverflow.com/questions/34608826/what-is-the-impact-of-the-requires-full-screen-option-in-xcode-for-an-iphone-o)
