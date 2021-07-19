---
title: ios) Notification Center 사용방법
author: cotchan
date: 2021-07-19 09:49:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Notification Center

+ NotificationCenter 에 등록된 event 가 발생하면 해당 event에 대한 행동을 취합니다.

![스크린샷 2021-07-19 오전 9 36 52](https://user-images.githubusercontent.com/75410527/126100319-40968d16-d09b-4e2e-8cad-651fce8722a0.png)

---

## 2. 객체 얻어오는 방법


```swift
//NotificationCenter Singleton Pattern
NotificationCenter.default
```

---

## 3. addObserver

+ **옵저버가 탐지하는 대상은 `name`이라는 키 값을 탐지합니다.**
  + **예를 들어, 어떤 객체가 `myNoti`라는 name으로 notification을 전송하면** 그 이벤트를 탐지하게 됩니다.

```swift
//정의
NotificationCenter.default.addObserver(observer:Any, selector:Selector, name: NSNotification.Name?, object:Any?)
```

```swift
//예시: myNoti라는 name의 notification이 오면 selector를 실행하라 라는 뜻
NotificationCenter.default.addObserver(self, selector: #selector(handleNoti(_:)), name: myNoti, object: nil)

@objc func handleNoti(_ noti: Notification) {
    print(noti)
    print("handleNoti")
}
```

---

+ **selector의 handleNoti를 실행하면, 파라미터로 `post`메서드가 보낸 아래의 값들이 넘어옵니다.**
  + name = myNoti
  + object = Optional(전달할 값)
  + userInfo = nil 값이 넘어오게 됩니다.

---

## 4. post

+ post는 이벤트를 발생시키는 부분입니다. 즉, 노티피케이션을 발송하는 부분입니다.
+ **`myNoti` 이벤트를 발생시키면서 Observer에게 전달할 값(선택사항)을 포함해서 전달해줍니다.**

```swift
NotificationCenter.default.post(name:"myNoti", object:" 전달할 값")
NotificationCenter.default.post(name: NSNotification.Name("TestNotification"), object: nil, userInfo: nil)​
```

---

## 5. removeObserver

+ NotificationCenter는 싱글턴 인스턴스라서 여러 오브젝트에서 공유합니다. 
+ 그래서 옵저버를 등록한 오브젝트가 메모리에서 해제되면 NSNotificationCenter에서도 옵저버를 없앴다고 알려줘야 됩니다.
+ **보통 `deinit`에서 호출합니다.**

```swift
//self에 등록된 옵저버 전체 제거
notiCenter.removeObserver(self) 

//옵저버 하나 제거(등록된 특정 노티를 제거하기 위해 사용)
notiCenter.removeObserver(self, name: NSNotification.Name.UIKeyboardDidShow, object: nil)
```


---

+ 출처
  + [[iOS/Swift] NotificationCenter 사용하기](https://silver-g-0114.tistory.com/106)
  + [[iOS] NotificationCenter](https://jinshine.github.io/2018/07/05/iOS/NotificationCenter/)
