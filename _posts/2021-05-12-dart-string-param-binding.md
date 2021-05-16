---
title: dart) String Parameter Binding ${}
author: cotchan
date: 2021-05-12 11:15:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. $

---

## 2. ${}

+ dart에서 스트링 내부에 변수값을 포함하는 방법은 아래와 같습니디.

```dart
// Dart supports string interpolation.
// String 안에 변수값 포함하고 싶을 때
const pi = 3.14;
print('pi is $pi');

// print('pi is' + pi) 도 가능
// {} 있는 이유는 복잡한 계산, object안의 변수 값을 들여다 볼 때 필요
print('tau is ${2 * pi}');
```

---

+ 출처
  + [[Dart] 기초개념](https://velog.io/@jinsk9268/Dart-1)

