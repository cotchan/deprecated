---
title: dart) 상속 관계 사용방법
author: cotchan
date: 2021-04-09 22:30:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. 상속받은 자식 클래스 초기화 방법

+ **`super` 키워드를 사용합니다.**

---

## 1-1. 예시 1

```dart
class MyClass1 {
  double _myPrivateVar;
  MyClass1([double myPrivateValue]) : _myPrivateVar = myPrivateValue;
}
// In different library:
class MyClass2 extends MyClass1 {
  MyClass2(double myVar) : super(myVar);
}
```

---

## 1-2. 예시 2

+ 부모클래스 `RequestDto`와 **자식 클래스 `LoginRequestDto`, `AutoLoginRequestDto` 초기화 방법**

```dart
abstract class RequestDto {

  final String host;
  RequestDto(this.host);
}
```

---

```dart
class LoginRequestDto extends RequestDto {

  final String userId;
  final String password;

  LoginRequestDto._builder(LoginRequestDtoBuilder builder) :
        userId = builder.userId,
        password = builder.password,
        super(builder.host);
}
```  

---

```dart
class AutoLoginRequestDto extends RequestDto {

  final String token;

  AutoLoginRequestDto(String requestUrl, String accessToken) :
      token = accessToken,
      super(requestUrl);
}
```

---

+ 출처
  + [In Dart, how to access a parent private var from a subclass in another file?](https://stackoverflow.com/questions/58715755/in-dart-how-to-access-a-parent-private-var-from-a-subclass-in-another-file)
  + [다트 상속(Dart Inheritance)](https://brunch.co.kr/@mystoryg/125)
