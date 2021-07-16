---
title: ios) 현재 앱의 최상단 뷰 컨트롤러 얻기
author: cotchan
date: 2021-07-16 13:55:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. UIApplication.shared.windows

+ 현재 실행중인 뷰 컨트롤러가 뭔지 얻는 방법입니다.

```swift
guard let keyWindow = UIApplication.shared.windows.filter({ $0.isKeyWindow }).first else {
    return
}

keyWindow.rootViewController?.present(nextViewController, animated: false) {
    //...
}
```

---

## 2. UIApplication Extension화

+ 이 코드를 사용하면 `UIApplication.topViewController()`를 통해 현재 뷰 컨트롤러에 접근할 수 있습니다.

```swift
extension UIApplication {
    class func topViewController(base: UIViewController? = UIApplication.shared.keyWindow?.rootViewController) -> UIViewController? {
        if let nav = base as? UINavigationController {
            return topViewController(base: nav.visibleViewController)
        }
        if let tab = base as? UITabBarController {
            if let selected = tab.selectedViewController {
                return topViewController(base: selected)
            }
        }
        if let presented = base?.presentedViewController {
            return topViewController(base: presented)
        }
        return base
    }
}
```




---

+ 출처
  + [[SWIFT] 현재 실행 중(혹은 실행할) 앱의 최상 뷰 컨트롤러 얻기](https://g-y-e-o-m.tistory.com/93)
