---
title: ios) UIScreen.main.bounds 
author: cotchan 
date: 2021-06-25 15:30:21 +0800 
categories: [ios] 
tags: [ios] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. UIScreen

+ **UIScreen을 통해 디스플레이(하드웨어)의 정보를 가져올 수 있습니다.**
+ UIViewController의 Bounds 또는 Frame 값을 가져올 수도 있지만, UINavigationBar 또는 SafeArea 때문에 화면의 크기를 제대로 가져올 수 없습니다.

```swift
//ex1
let bounds: CGRect = UIScreen.main.bounds

//ex2
private var watermarkView: UIStackView?

watermarkView.frame = UIScreen.main.bounds
```


---

+ 출처
  + [[iOS] UIScreen으로 화면 크기(해상도) 가져오기](https://mildwhale.tistory.com/14)
