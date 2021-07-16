---
title: Swift2) 싱글톤 패턴
author: cotchan 
date: 2021-07-16 09:52:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **아래 출처를 참고하여 개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. Swift 싱글톤 사용방법

+ **`싱글톤 객체 선언 방법`**

```swift
class UserInfo {
    static let shared = UserInfo()

    //...
    var id: String?
    var password: String?
    var name: String?

    private init() { }
}
```

+ **외부에서 사용할 때**

```swift
//A ViewController
let userInfo = UserInfo.shared
userInfo.id = "Sodeul"

//B ViewController
let userInfo = UserInfo.shared
userInfo.password = "123"

//C ViewController
let userInfo = UserInfo.shared
userInfo.name = "Sodeul"
```

---

## 2. Swift Singleton 장점

+ **`Objective-C`에서 Singleton을 생성할 땐, `dispatch_once`를 이용해 단 한번만 불리게 하는 작업이 있습니다.**
  + 그 이유는 멀티 쓰레드환경에서 싱글턴 객체 생성이 `Thread-Safe`하지 않기 때문입니다.
    + 여러 쓰레드가 동시에 싱글턴 객체를 생성하게 되면 싱글턴 인스턴스가 여러 개 생성될 수 있습니다.

```ruby
@interface UserInfo : NSObject
 
+ (instancetype)sharedInstance;
 
@end
 
 
@implementation UserInfo
 
+ (instancetype)sharedInstance {
    static UserInfo *shared = nil;
 
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        shared = [[UserInfo alloc] init];
    });
 
    return shared;
}
 
@end
```

+ **그러나 Swift의 경우 static을 사용해 타입 프로퍼티로 인스턴스를 생성하면, `사용 시점에 초기화(lazy)`가 됩니다.**
+ **그러므로 Singleton Instance가 최초 생성되기 전까진 메모리에 올라가지 않고 `Dispatch_once도 자동 적용`됩니다.**
  + 따라서 별 코드 없이도 인스턴스가 여러 개 생성되지 않는 `Thread-Safe`한 방법이 됩니다.

---

+ 출처
  + [Swift) 싱글톤 패턴(Singleton Pattern)](https://babbab2.tistory.com/66)
