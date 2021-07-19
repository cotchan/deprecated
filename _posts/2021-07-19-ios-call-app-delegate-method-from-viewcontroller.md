---
title: ios) Call app delegate method from view controller
author: cotchan
date: 2021-07-19 09:23:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

```swift
let appDelegate: AppDelegate? = UIApplication.shared.delegate as? AppDelegate
appDelegate?.anyAppDelegateInstaceMethod()
```

---

+ 출처
  + [Call app delegate method from view controller](https://stackoverflow.com/questions/30705214/call-app-delegate-method-from-view-controller)
