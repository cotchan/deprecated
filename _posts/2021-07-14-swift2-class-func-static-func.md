---
title: Swift2) class func vs static func
author: cotchan 
date: 2021-07-14 16:45:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. 공통점

+ **둘 다 `타입 메소드`로써 생성자를 통해 인스턴스를 생성하지 않더라도 바로 접근이 가능합니다.**

```swift
Sample.myClassFunc()

Sample.myStaticFunc()
```

---

## 2. 차이점

+ **둘의 차이점은 `오버로딩 가능/불가능 여부`입니다.**

```swift
class SubSample : Sample {

//Good!
override class func myClassFunc() {}

//Compile Error
override static func myStaticFunc() {}
}
```

---

## 2-1. class func

+ **`오버라이딩 가능`**
+ **그러므로 상속이 불가능한 `struct`이나 `enum`에서 class func을 정의하면 에러가 납니다.**

---

## 2-2. static func

+ **`오버라이딩 불가능`**



---

+ 출처
  + [[Swift] class func vs static func](https://sujinnaljin.medium.com/swift-class-func-vs-static-func-7e6feb264147)
