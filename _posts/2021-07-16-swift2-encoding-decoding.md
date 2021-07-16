---
title: Swift2) Encoding and Decoding
author: cotchan 
date: 2021-07-16 09:30:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **아래 출처를 참고하여 개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. Encoding 방법

---

## 2. Decoding 방법

---

## 2-1. Decodable

+ `Decodable`을 이용하면 JSON을 쉽게 처리할 수 있습니다.

+ **`decode` 메서드는 두개의 파라미터가 필요합니다.**
  + `첫 번째 파라미터`: 디코딩할 타입을 정의합니다.
    + **반드시 `Decodable`이나 `Codable` 프로토콜을 채택해야합니다.** 
  + `두 번째 파라미터`: json이 저장되어있는 Data를 전달합니다.

```swift
//첫 번째 파라미터

struct Person: Codable {
    var name: String
    var age: Int
    var birthDate: String
    var address: String?
}
```

```swift
//두 번째 파라미터

let jsonData =
"""
{
"name": "Jinnify",
"age": 29,
"birthDate": "2019-04-29T01:30:00Z",
"address": "동대문구 답십리"
}
""".data(using: .utf8)!
```

+ **최종적으로 decode하는 방법**

```swift
let decoder = JSONDecoder()

do {
    let result = try decoder.decode(Person.self, from: jsonData)
    print(result)
} catch {
    print(error)
}

/*
결과
Person(name: "Jinnify", age: 29, birthDate: "2019-04-29T01:30:00Z", address: Optional("동대문구 답십리"))
*/
```

---

## 2-2. JSON KEY와 동일하지 않은 경우

+ 위와 같이 JSON Key와 Codable을 따르는 프로퍼티 이름이 동일하면 간단하게 처리됩니다.
+ **하지만 JSON Key값과 Codable 프로퍼티의 이름을 다르게 하고자 하면 `CodingKeys`를 정의해주어야 합니다.**

```swift
//json data

let jsonData =
"""
{
"name": "Jinnify",
"age": 29,
"birth_date": "2019-04-29",
"address": "동대문구 답십리"
}
""".data(using: .utf8)!
```

```swift
//Codable 

struct Person: Codable {
    var name: String
    var age: Int
    var birthDate: String
    var juso: String?
    
    enum CodingKeys: String, CodingKey {
        case name, age
        case birthDate = "birth_date"
        case juso = "address"
    }
}
```

```swift

let decoder = JSONDecoder()

do {
    let result = try decoder.decode(Person.self, from: jsonData)
    print(result)
} catch {
    print(error)
}

/*
결과
Person(name: "Jinnify", age: 29, birthDate: "2019-04-29", juso: Optional("동대문구 답십리"))
*/
```

+ 그럼 이제 아래와 같이 접근이 가능합니다.

```swift

do {
    let result = try decoder.decode(Person.self, from: jsonData)
    result.juso
    result.birthDate
    result.age
    result.name
} catch {
}
```

+ **`snake_case`로 jsonData가 내려올경우 `lowerCamelCase`로만 변환하는 방법**

```swift
let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase
```

---

+ 출처
  + [[Swift]Codable - Decoding 방법](https://jinnify.tistory.com/71)
  + [Encoding and Decoding in Swift](https://www.raywenderlich.com/3418439-encoding-and-decoding-in-swift)
