---
title: Flutter) 연산자
author: cotchan
date: 2021-04-06 19:19:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트할 예정입니다.**

---

## 1. ?? (if null)

+ 널인지 여부 검사



```dart
//객체가 null이 아니면 길이를, null이면 0을 반환하는 코드
print(name?.length ?? 0); // name이 null이면 0을 출력
```

---

```dart
// assign y to x, unless y is null
//   otherwise assign z
x = y ?? z;

// assign y to x if x is null
x ??= y;

// call foo() only if x is not null
x?.foo();
```

---

+ 출처
  + [[FLUTTER] DART 언어 기초과정 - 2 / A Tour of the Dart Language](https://steemit.com/dart/@wonsama/flutter-dart-2-a-tour-of-the-dart-language)
  + [Null-aware operators in Dart
](http://blog.sethladd.com/2015/07/null-aware-operators-in-dart.html)
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

