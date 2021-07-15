---
title: Swift2) wild card 사용 방법(매개변수, 리턴값, 클로저 매개변수)
author: cotchan 
date: 2021-07-15 11:22:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. 매개변수에서 사용

+ **wildcard를 붙이면 해당 `parameter의 네이밍을 생략`할 수 있습니다.**

---

## 1-1. wildcard 적용 전

+ **swift는 기본적으로 parameter에 대한 네이밍(?)도 함께 전달해줘야합니다.**

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

+ 또한 아래와 같은 형태로 `오버라이딩하는 함수의 매개변수를 쓰지 않음을 표현`할 수도 있습니다.

```swift
class MyView: UIView {
    override func draw(_ _: CGRect) {
        // complete the draw without use of the CGRect
    }
}
``` 

---

## 2. return value 사용 안할 때

+ **return 값이 있는 함수에서 `return 값을 사용하지 않는 경우` wild card를 사용합니다.**

```swift
_ = addNumbers(3, 4)


//아래와 같이 쓰면 컴파일러 오류입니다.
addNumbers(3, 4)
```

---

## 3. 클로저 매개변수 대신 사용

+ **와일드 카드를 사용하여 Swift `클로저의 특정 매개변수를 건너뛸 수 있습니다.`**

```swift
let names = [ ("James", 17), ("Tom", 16), ("Robert", 18) ]

let result = names.filter { (name, _) in 
  if name == "Tom" {
    return false
  } else {
    return true
  } 
}

print (result) // returns everyone except Tom
```

---

+ 출처
  + [The Wildcard Pattern in Swift](https://medium.com/macoclock/the-wildcard-pattern-in-swift-8f0277350941)
