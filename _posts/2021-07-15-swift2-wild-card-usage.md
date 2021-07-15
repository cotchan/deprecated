---
title: Swift2) wild card 사용 방법(parameter)
author: cotchan 
date: 2021-07-15 11:22:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. parameter로 사용 시

+ **wildcard를 붙이면 해당 `parameter의 네이밍을 생략`할 수 있습니다.**

---

## 1-1. wildcard 적용 전

```swift
func testMethod(forOuter forInner: String, forOuter2 forInner2: String) -> String  {
    return forInner + forInner2
}

print(testMethod(forOuter: "abcde", forOuter2: "xyz"))  //abcdexyz
```

---

## 1-2. wildcard 적용 후

```swift
func testMethod2(_ forInner: String, _ forInner2: String) -> String  {
    return forInner + forInner2
}

print(testMethod2("abcde", "xyz"))  //abcdexyz
```

---

+ 출처
  + []()
  + []()
