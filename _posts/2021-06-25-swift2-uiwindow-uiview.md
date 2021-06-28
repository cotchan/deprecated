---
title: Swift2) UIWindow, UIView
author: cotchan 
date: 2021-06-25 15:08:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. UIWindow

+ **`앱의 UI 배경과 이벤트를 뷰로 전달하는 객체`**
+ **UIWindow는 사용자 인터페이스에 `배경(backdrop)을 제공`하고, 중요한 `이벤트 처리(Behaviors)를 제공`하는 객체**
  + UIWindow는 뷰 컨트롤러와 함께 이벤트를 처리하고 앱 동작에 필수적인 작업을 수행합니다.
+ **UIWindow를 사용하는 경우**
  + 앱의 콘텐츠를 표시할 기본 window를 제공
  + 추가 콘텐츠를 표시할 때 필요한 경우 추가 window를 생성

+ 또한 UIWindow는 시각적 모양은 없고 `Root View Controller에서 관리하는 하나 이상의 뷰를 보여주는 역할`을 합니다.

---

## 2. Window에 콘텐츠 추가하는 법

+ 아래와 같이 `addSubview()` 메서드를 사용하여 뷰를 추가합니다.
+ 또는 `rootViewController` 프로퍼티를 사용할 수도 있습니다.
  + rootViewController 프로퍼티는 루트 뷰를 설정하는데만 사용됩니다.

```swift
func addWindowComponent(window: UIWindow) {

    // window에 콘텐츠 추가 방법1
    window.addSubview(UIView())

    // window에 콘텐츠 추가 방법2
    window.rootViewController = ViewController()
}
```


---

+ 출처
  + [iOS ) UIWindow. 그리고 UIView](https://zeddios.tistory.com/283)
  + [[iOS 앱개발] UIWindow 알아보기](https://icksw.tistory.com/140)
