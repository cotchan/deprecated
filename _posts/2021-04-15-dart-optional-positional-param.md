---
title: dart) 함수 (Optional param, Positional param)
author: cotchan
date: 2021-04-15 15:43:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. Optional param {}

+ **`선택적 매개변수`**
  + 선택적 파라미터의 사용 방식은 `{param1, param2, …} 형태`로 선언 합니다.
  + **`꼭 필수로 입력받아야하는 매개변수가 있다면` `@required` 어노테이션(Annotation)을 이용합니다.**
  + 단 @required 어노테이션을 사용하려면 meta 패키지를 설치하고 import 해야됩니다.


```dart
enableFlags(bold: true, hidden: false);

/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold, bool hidden}) {...}

const Scrollbar({Key key, @required Widget child})


String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}


assert(say('Bob', 'Howdy') == 'Bob says Howdy');

assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

---

## 2. Positional param []

+ **`위치 매개변수`**
  + **선택적으로 매개변수를 생략하기를 원한다면** 위치 매개변수를 사용하면 됩니다. 
  + **해당 매개변수들을 `괄호[]로 감싸면 됩니다.`**

```dart
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}

assert(say('Bob', 'Howdy') == 'Bob says Howdy');

assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

---

+ 출처
  + [[Dart] Functions(함수)](https://joycestudios.tistory.com/73)
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)
