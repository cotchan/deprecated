---
title: Swift2) weak var
author: cotchan 
date: 2021-07-22 09:54:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 아래 출처를 참고하여 작성하였습니다.
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. weak (약한 참조)

+ 해당 인스턴스의 소유권을 가지지 않고, 주소값만을 가지는 `포인터 개념`입니다.
+ 자신이 참조하는 인스턴스의 retain count를 증가시키지 않습니다. release도 발생하지 않습니다.
+ 자신이 참조는 하지만 weak 메모리를 해제시킬 수 있는 권한은 다른 클래스에 있습니다.
+ **메모리가 해제될 경우 자동으로 레퍼런스를 `nil`로 초기화 해줍니다.**
+ **weak 속성을 사용하는 객체는 `항상 optional 타입`이어야 합니다.(해당 객체가 nil일 수 있기 때문)**



---

## 2. strong (강한 참조)

+ 해당 인스턴스의 소유권을 가집니다.
+ 자신이 참조하는 인스턴스의 retain count를 증가시킵니다.
+ 값 지정 시점에 retain이 되고 참조가 종료되는 시점에 release가 됩니다.
+ **선언할 때 아무것도 적어주지 않으면 `default로 strong`이 됩니다.**

---

## 3. 어떤 상황에 쓰는가?

+ **strong**
  + 레퍼런스 카운트를 증가시켜 ARC로 인한 메모리 해제를 피하고, 객체를 안전하게 사용하고자 할 때 쓰입니다.
+ **weak**
  + 대표적으로 retain cycle에 의해 메모리가 누수되는 문제를 막기 위해 사용되며, **`delegate` 패턴에서 사용합니다.**

---

## 4. delegate 순환참조 해결

+ **순환참조란**
  + 서로가 서로를 소유하고 있어 절대 메모리가 해제되지 않는 상황

+ delegate 패턴을 사용하기 위해서는 일을 시키는 객체와 일을 하는 객체 두 개가 무조건 있어야 합니다.

+ `일을 하는 객체가 일을 시키는 객체를 만들고`, 자신을 일을 시키는 객체에 연결시켜줌으로써 일을 하는 객체는 `일을 시키는 객체를 소유`하게 됩니다.

```swift
//일을 하는 애 구현
//Receiver 구현

class Receiver: DelegateProtocol {
    let button = ObjectNeedingDelegate()
    
    init() {
        button.delegate = self  //대리자(일을 시키는 애)에게 자기 자신을 전달
    }
    
    func helloWorld() {
        print("Hello World")
    }
}
```

---

+ 여기서 일을 시키는 객체도 일을 하는 객체를 소유하게 되버리면 둘 사이에 순환참조가 발생하게 됩니다.
+ **이 문제를 해결하기 위해 `일을 시키는 객체에서는` `일을 하는 객체를 weak로 참조`합니다.**
  + 그러면 일을 하는 객체만 일을 시키는 객체를 소유하게 되기 때문에 순환참조가 발생하지 않습니다.

```swift
//일을 시키는 애 구현
//Delegate 구현
//Sender 구현

class ObjectNeedingDelegate {
    weak var delegate: DelegateProtocol?
    
    func didTapButton() {
        delegate?.helloWorld()
    }
}
```

---

+ 출처
  + [[Swift] 메모리를 참조하는 방법(Strong, Weak, Unowned)](https://devsrkim.tistory.com/entry/Swift-%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%A5%BC-%EC%B0%B8%EC%A1%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-Strong-Weak-Unowned)
