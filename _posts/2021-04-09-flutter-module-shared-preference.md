---
title: Flutter) SharedPreferences 사용 방법
author: cotchan
date: 2021-04-09 22:00:00 +0800
categories: [Flutter, Flutter_module]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 라이브러리 import

```dart
//pubspec.yaml

dependencies:
  flutter:
    sdk: flutter

  # shared_preferences
  shared_preferences: 0.5.8
```

---

## 2. 사용 방법


---

## 2-1. insert


```dart
/// 자동로그인 정보를 수정함
Future<void> saveUser(User user) async {

  final prefs = await SharedPreferences.getInstance();

  prefs.setString('userHost', user.host);
  prefs.setString('userToken', user.token);
}
```

---

## 2-2. delete

```dart
  Future<void> deleteUser() async {
    final prefs = await SharedPreferences.getInstance();
    prefs.remove('userHost');
    prefs.remove('userToken');
  }
```

---

## 2-3. get


```dart
  Future<Map> getUser() async {
    final prefs = await SharedPreferences.getInstance();
    return {'userHost': prefs.get('userHost'), 'userToken': prefs.get('userToken')};
  }
```

---

## 2-4. 예시 Domain Entity

+ **CRUD 예시를 위해 사용한 `User Entity`**

```dart
//user.dart
class User {

  //인증 토큰
  final String token;

  //인증 토큰 보낼 URL
  final String host;
}
```

---

## 3. 사용하며 겪은 에러사항

---

## 3-1. MissingPluginException

+ **Shared Preference를 사용하려고 하면 아래 코드 부분에서 `MissingPluginException` 발생**

```dart
//예외가 발생하는 지점
SharedPreferences prefs = await SharedPreferences.getInstance();
```

```dart
//Error Message
Flutter: Unhandled exception: MissingPluginException(No implementation found for method getAll on channel plugins.flutter.io/shared_preferences)
```

---

+ **해결방법: SharedPreferences `사용하는 곳에 아래 한 줄 추가`**

```dart
SharedPreferences.setMockInitialValues({});
```

---

+ 출처
  + [shared_preferences](https://pub.dev/packages/shared_preferences)
  + [Flutter: Unhandled exception: MissingPluginException(No implementation found for method getAll on channel plugins.flutter.io/shared_preferences)](https://stackoverflow.com/questions/50687801/flutter-unhandled-exception-missingpluginexceptionno-implementation-found-for)
