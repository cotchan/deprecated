---
title: ios) AppDelegate의 역할(feat. UIWindow)
author: cotchan
date: 2021-07-16 13:25:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 아래 출처를 참고하여 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. AppDelegate 역할

+ `UITableViewDelegate`를 구현하다 => `UITableView`에서 필요한 기능을 대신 구현하다

+ **위와 같은 의미처럼 `AppDelegate`는 `App(Application)이 해야할 일을 대신 구현한다는 의미`입니다.**

+ **여기서 App이 해야할 일이란, Background 진입, Foreground 진입, 외부에서의 요청 등을 말합니다.**

---

## 2. @UIApplicationMain

+ **실제로 앱은 UIApplication이라는 객체로 추상화되어 Run Loop를 통해 프로그램 코드를 실행합니다.**
+ 개발자는 AppDelegate를 통해 UIApplication의 역할의 일부를 위임받아 UI를 그리면서 앱이 탄생합니다.

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate
```

---

## 3. UIWindow

```swift
var window: UIWindow?
```

+ AppDelegate 클래스를 보면 window 변수가 하나 있습니다. 
  + 스토리보드 기반의 앱으로 구동되면 시스템에서 UIWindow 객체를 할당하여 초기화합니다.
  + 코드기반의 앱으로 구동되면 `didFinishLaunchingWithOptions` 타이밍에 직접 생성해줘야합니다.
+ **UIWindow는 View를 담는 컨테이너 역할입니다.**

---

+ 출처
  + [[iOS] AppDelegate의 역할과 메소드](http://monibu1548.github.io/2018/08/28/appdelegate/)
