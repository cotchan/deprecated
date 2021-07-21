---
title: Swift2) Result 타입 (feat. .success, .failure)
author: cotchan 
date: 2021-07-21 15:39:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ **아래 출처를 참고하여 작성하였습니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. Result 타입이란

+ **Swift의 Result 타입은 `success`와 `failure` 두 가지 case가 있는 `enum`입니다.**
+ Result 타입은 비동기 API와 같은 복잡한 코드에서 에러를 보다 간단하고 명확하게 처리할 수 있게 도와줍니다.

---

## 1-1. .success

+ 제네릭을 사용하여 구현되므로 개발자가 정한 타입의 연관값을 가질 수 있습니다.

---

## 1-2. .failure

+ 제네릭을 사용하여 구현되므로 개발자가 정한 타입의 연관값을 가질 수 있습니다.
+ **단, `failure`의 연관값은 Swift의 `Error 타입을 채택한 타입`이어야 합니다.**

```swift
enum NetworkError: Error {
    case badURL
}

public enum APIError: Error {
    case failed(code: String, message: String)
}

//아래와 같이 호출합니다.
throw APIError.failed(code: "Invalid Data", message: "Invalid Data")
```

---

## 2. 사용 예제

+ Result를 실제로 사용하는 예제를 위해 서버에 연결하는 함수를 작성해 봅시다. 이 함수는 읽지 않은 메시지의 수를 서버에서 가져와 사용자에게 알려주는 작업을 수행합니다. 이 예제에서는 요청한 URL이 유효한 URL이 아닌 경우를 표현하는 에러 하나만 정의합니다.


+ 결과를 가져 오는 함수는 첫 번째 인자로 URL 문자열을 받고 두 번째 인자로 completion 핸들러를 받습니다. 
+ completion 핸들러는 `Result`를 인자로 받습니다. 
  + **여기서 `success case`는 `정수`를 연관값으로 받습니다.**
  + **여기서 `failure case`는 `NetworkError` 중 하나를 연관값으로 받습니다.**

---

## 2-1. Result completionHandler 정의

```swift
import Foundation

func fetchUnreadCount1(from urlString: String, completionHandler: @escaping (Result<Int, NetworkError>) -> Void)  {
    guard let url = URL(string: urlString) else {
        completionHandler(.failure(.badURL))
        return
    }

    // complicated networking code here
    print("Fetching (url.absoluteString)...")
    completionHandler(.success(5))
}
```

---

## 2-2. Result completionHandler 사용

+ **이 함수를 사용할 때는 `Result 값을 확인`하여 함수 호출이 성공했는지 실패했는지 확인해야 합니다.**

```swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in
    switch result {
    case .success(let count):
        print("(count) unread messages.")
    case .failure(let error):
        print(error.localizedDescription)
    }
}
```

---

## 3. Result 추가 사항

+ Result 타입을 실제로 사용하기 전에 알아야 할 세 가지 내용이 있습니다.

---

## 3-1. get() 메서드

+ **Result 타입에는 `get()` 메서드가 있습니다.**
+ 이 메서드는 성공한 값이 있으면 반환하고 그렇지 않으면 에러를 throw합니다. 
+ get() 메서드를 사용하면 Result 타입 사용을 다음과 같이 일반적인 예외 던지기로 변환 할 수 있습니다.

```swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in
    if let count = try? result.get() {
        print("(count) unread messages.")
    }
}
```

---

## 3-2. throwing closure 초기자

+ **Result 타입에는 `throwing closure를 받는 초기자`가 있습니다.**
+ 클로저가 값을 성공적으로 반환하면 `success case`에서 `그 값을 연관값으로 저장`하고 그렇지 않으면 `throw 된 에러`를 `failure case`의 `연관값으로 저장`합니다.

+ 예를 들어 다음 코드를 실행하면, 성공일 경우 someFile의 내용이 success의 연관값으로 담겨 result에 저장되고 실패일 경우 에러가 failure의 연관값으로 담겨 result에 저장됩니다.

```swift
let result = Result { try String(contentsOfFile: someFile) }
```

---


## 3-3. Swift Error 프로토콜 사용

+ **failure 케이스에 개발자가 정의한 특정 Error enum을 사용하는 대신 `Swift의 Error 프로토콜을 사용`할 수도 있습니다.**
+ 따라서 `Result<Int, NetwordError>`를 사용하는 대신 `Result<Int, Error>`를 사용할 수 있습니다. 
+ 이렇게 하면 타입이 지정된 throw의 타입 안전성을 잃게 되지만 그대신 다양한 Error enum을 사용할 수 있습니다. 
+ 위 제안을 따르냐 마느냐는 원하는 코딩 스타일에 따라 유연하게 선택하면 됩니다.

---

+ 출처
  + [Result 타입](https://tech.burt.pe.kr/swift/what-new/swift-5.0/result-type)
