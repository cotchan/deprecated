---
title: TIL) Swift Escaping Closure
author: cotchan
date: 2021-07-23 10:00:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 탈출 클로저 핵심 개념

+ **탈출 클로저의 핵심은 말 그대로 `전달인자로 받은 클로저가` `함수 내부 scope 안에서 실행되는 것이 아니라` 이를 탈출해서 `다른 어딘가로 가는 것` 입니다.**


+ 아래와 같은 경우에 탈출한다고 볼 수 있습니다.
  + **탈출 클로저가 외부 글로벌 변수에 저장되는 경우**
  + 전달받은 클로저가 클로저 함수 외부로 다시 반환

---

## 2. 탈출 클로저 사용 순서

1. 탈출 클로저를 parameter로 받는 함수가 존재합니다. 
  + **위 함수를 `1번 타입 메서드`라고 부르겠습니다.**
  + 위 함수를 사용하는 것부터가 시작입니다.
2. **외부에서 1번 타입 메서드를 호출하면서 parameter로 `임의의 탈출 클로저를 정의해서 넣어줍니다.`**
3. **1번 타입 메서드 내부 어딘가에서(외부 글로벌 변수 등) `parameter로 받은 탈출클로저를 저장해놓습니다.`**
  + 저장 목적은 나중에 다른 데서 호출해서 쓰기 위함입니다.
4. **1번 타입 메서드 내부 어딘가에서 `저장했던 탈출 클로저를 호출합니다.`**
  + 이 시점에 실제로 2번 과정에서 제공한 탈출 클로저가 실행됩니다.

---

## 3. Sample Code

```swift
typealias CompletionHandler = (Int, Bool) -> Void


class UsingClosureClass {
    
    static let shared = UsingClosureClass()
    
    private var completionBlock: CompletionHandler?
    
    func register(completion: @escaping CompletionHandler) {
        //전달인자로 받은 탈출 클로저를 외부 글로벌 변수에 저장합니다.
        //목적은 나중에 다른데서 이 탈출 클로저를 호출한다는 뜻입니다.
        self.completionBlock = completion
    }
    
    //이 메서드에서 외부 글로벌 변수로 저장했던 탈출 클로저를 호출합니다.
    func useEscapingClosure() {
        completionBlock?(4, true)
    }
}

class SupplyClosureClass {
    
    static let shared = SupplyClosureClass()
    
    func supplyEscapingClosure() {
        //이 메서드에서 전달 인자(parameter)로 탈출 클로저를 '제공'해줍니다.
        UsingClosureClass.shared.register { [weak self] whatIsThis, whatIsThis2 in
            if whatIsThis2 {
                print("this is if case. \(whatIsThis)")
            } else {
                print("this is else case. \(whatIsThis)")
            }
        }
    }
}


SupplyClosureClass.shared.supplyEscapingClosure()

//wait
for _ in 0...2 {
    sleep(1)
}

UsingClosureClass.shared.useEscapingClosure()


// Result
// this is if case. 4
```

---

+ 출처
  + [Swift - Escaping Closure(탈출 클로저) 간단 이해](https://jinsangjin.tistory.com/99)
