---
title: Swift2) Delegate Patern
author: cotchan 
date: 2021-07-21 17:39:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ **아래 출처를 참고하여 작성하였습니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. 세 가지 컴포넌트

+ **`DelegateProtocol`**
  + 요구사항에 해당하는 기능(서비스) 입니다.
+ **`일을 시키는 컴포넌트`**
  + 아래와 같이 불립니다.
    + `Sender`
    + `Delegate`
    + `대리자`
    + `위임자`
+ **`일을 하는 컴포넌트`**
  + 아래와 같이 불립니다.
    + `Receiver`
    + `수신자`

---

## 2. 일을 시키는 애(Delegate)

+ Object Needing a Delegate 입니다.
+ **`가장 큰 특징은`, 일을 시키기 위해서는 요구사항에 해당하는 기능을 알아야하므로 `Delegate Protocol을 소유`합니다.**
+ **클라이언트에 해당하며 `DelegateProtocol 서비스를 사용하는 쪽`입니다.**

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

## 3. 일을 하는 애(Receiver)

+ Object Acting as a Delegate 입니다.
+ **`Delegate Protocol을 구현한 클래스`입니다.**
+ **`가장 큰 특징은`, 일을 하는 애(Receiver)는 `일을 시키는 애(Delegate)에게` `자기 자신을 전달`합니다.**

```swift
//요구사항에 해당하는 protocol 디자인
protocol DelegateProtocol {
    func helloWorld()
}
```

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

+ 출처
  + [[Swift] Delegation Pattern](https://baked-corn.tistory.com/23)
  + [[Swift] - Delegate Pattern에 대해서](https://velog.io/@iwwuf7/Swift-Delegate-Pattern%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)
  + [Design-pattern) Delegation Pattern](https://cotchan.github.io/posts/design-pattern-delegate-pattern/)
