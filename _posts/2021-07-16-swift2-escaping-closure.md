---
title: Swift2) Escaping 클로저(@escaping)
author: cotchan 
date: 2021-07-16 09:20:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **아래 출처 글을 참여하여 개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. Escaping 클로저란

+ Escaping 클로저는 클로저가 함수의 인자로 전달됐을 때, **`함수의 실행이 종료된 후 실행되는 클로저`입니다.**

```swift
class ViewModel {
    var completionhandler: (() -> Void)? = nil
    
    func fetchData(completion: @escaping () -> Void) {
        completionhandler = completion
    }
}
```

+ **클로저가 실행되는 순서**

1. 클로저가 `fetchData()` 함수의 `completion` 인자로 전달됨
2. 클로저 `completion`이 `completionhandler` 변수에 저장됨
3. `fetchData()` 함수가 값을 반환하고 종료됨
4. 클로저 `completion`은 아직 실행되지 않음

+ `completion`은 함수의 실행이 종료되기 전에 실행되지 않기 때문에, 함수 밖(escaping)에서 실행되는 클로저입니다.

---

## 2. 사용하는 경우

+ **보통 `클로저가 다른 변수에 저장되어 나중에 실행되거나` `비동기로 실행될 때` `escaping` 클로저가 사용됩니다.**
+ escaping 클로저가 사용되는 흔한 예로는 비동기로 실행되는 HTTP Request CompletionHandler가 있습니다. 

```swift
func makeRequest(_ completion: @escaping (Result<(Data, URLResponse), Error>) -> Void) {
  URLSession.shared.dataTask(with: URL(string: "http://jusung.github.io/")!) { data, response, error in
    if let error = error {
      completion(.failure(error))
    } else if let data = data, let response = response {
      completion(.success((data, response)))
    }
  }
}
```

---

+ 출처
  + [[Swift] Escaping 클로저 (@escaping)](https://jusung.github.io/Escaping-Closure/)
