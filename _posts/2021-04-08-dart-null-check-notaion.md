---
title: dart) ?? 연산자(null check)
author: cotchan
date: 2021-04-08 22:40:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. '좌항' ?? '우항'의 형태
 
+ **`좌항 ?? 우항` 형태의 연산자 입니다.**
+ 좌항이 `null이 아니면 좌항을 리턴`하고 `null이면 우항을 리턴` 합니다.

```dart
// 좌항이 null인 경우
main(){
    String name;

    // 비어있습니다.
    print(name ?? "비어있습니다."); 
}
```

```dart
//좌항이 null이 아닌 경우
main(){
    String name;

    name = "sneakstarberry"

    // sneakstarberry
    print(name ?? "비어있습니다."); 
}
```

---

## 2. ?? (if null)

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
  + [[DART]다트(3) - 특이한 연산자](https://sneakstarberry.github.io/posts/dart03-operand/)
