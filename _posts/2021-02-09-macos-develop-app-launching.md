---
title: macOS) applicationWillFinishLaunching, applicationDidFinishLaunching란?
author: cotchan
date: 2021-02-09 09:12:21 +0800
categories: [Macos, Macos_Develop]
tags: [macos]
---

## 0. INTRO

+ Objective-C, Swift 개발 시 공통적으로 존재하는 메서드입니다.
+ **호출순서**
  1. **application`Will`FinishLaunching**
  2. **application`Did`FinishLaunching**

---

## 1. applicationWillFinishLaunching

+ **애플리케이션 개체가 `초기화되기 전에`** 기본 알림 센터에서 전송됩니다.
+ Sent by the default notification center immediately **before the application object is initialized.**
---

## 2. applicationDidFinishLaunching

+ **애플리케이션이 시작되고 `초기화 된 후` 첫 번째 이벤트를 받기 전에** 기본 알림센터에서 보냅니다.
+ Tells the delegate **when the app has finished launching.**

---

+ 출처
  + [applicationWillFinishLaunching](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428623-applicationwillfinishlaunching?language=objc)
  + [applicationDidFinishLaunching](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623053-applicationdidfinishlaunching?language=objc)


