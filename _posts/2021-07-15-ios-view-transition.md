---
title: ios) 화면 전환 방법(feat. VC 만들기, VC 전환)
author: cotchan
date: 2021-07-15 15:41:00 +0800
categories: [ios]
tags: [ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. uvc 직접 호출(present)

+ 가정
  + vc1와 vc2가 있다고 가정하고, vc1에서 vc2로의 화면 전환을 합니다.
  + 기본적인 개념은 VC1 위에 VC2가 생기고 VC2에서 VC1으로 돌아갈 땐 띄어진 VC2를 제거한다고 보면 됩니다.


```
present(<새로운 뷰컨트롤러 인스턴스>, animated:<애니메이션 여부>)
```

+ **`클로저를 추가하는 경우`는 다음과 같이 `completion` 매개 변수를 추가해주면 됩니다.**

```swift
present(_:animated:completion:)
```

+ 복귀하는 방법

```swift
dismiss(animated:)
```

---

## 1-1. 사용 예시

+ **먼저 스토리보드에서 viewController `Storyboard ID` 할당해줍니다.**

---

+ **`vc1 -> vc2로 이동`하기**

```swift
//vc1.swift

//스토리보드 내 특정 뷰컨트롤러를 찾는 메소드
guard let vc2 = self.storyboard?.instantiateViewController(withIdentifier: "${UVC_STORYBOARD_ID}") else{
	return
}

//uvc로 이동합니다. 애니메이션을 사용한다면 True 안한다면 False
self.present(vc2, animated: true)
```

---

+ **vc2 -> vc1로 `되돌아오기`**

```swift
//vc2.swift

self.presentingViewController?.dismiss(animated: true)
```

+ **`self.dismiss()`가 아니고 `self.presentingViewController`인 이유는 `제거하는 주체가 앞서 말했 듯 본인 자신이 아니고 VC1이기 때문입니다.`**

---

## 2. 네비게이션 컨트롤러 이용

+ **네비게이션 컨트롤러를 이용한 방식은 uvc를 직접 호출하는 것과 달리 `스택을 쌓아서 화면을 보여주는 방식`입니다.**

```swift
//새로운 화면 추가
pushViewController(_:animated:)

//화면 제거
popViewController(animated:)
```

---

## 2-1. 사용 예시

```swift
//vc1.swift

guard let uvc = self.storyboard?.instantiateViewController(withIdentifier: "${UVC_STORYBOARD_ID}") else {
    return
}

//push
self.navigationController?.pushViewController(uvc, animated: true)
```

```swift
//vc2.swift

//pop
_ = self.navigationController?.popViewController(animated: true)
```

+ **`self.navigationController`는 만약 현재의 뷰 컨트롤러에 네비게이션이 연결되어있지 않다면 nil 값을 반환하므로 옵셔널 값을 줍니다.**

---

## 3. 세그웨이를 이용한 방식

---

+ 출처
  + [Swift 화면전환-ViewController 직접 호출](https://lazyowl.tistory.com/entry/Swift-%ED%99%94%EB%A9%B4%EC%A0%84%ED%99%98-ViewController-%EC%A7%81%EC%A0%91-%ED%98%B8%EC%B6%9C)
