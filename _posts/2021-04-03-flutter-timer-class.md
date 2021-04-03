---
title: Flutter) Timer 클래스
author: cotchan
date: 2021-04-03 22:30:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Timer 클래스란

+ `Timer` 클래스는 `dart:async` 패키지에 포함된 클래스
+ **`일정 간격 동안 반복하여 동작을 수행`해야 할 때 사용하는 클래스입니다.**

--

## 2. 코드 예시

+ 다음 코드는 0.01초에 한 번씩 작업을 수행합니다.

```dart
import 'dart:async';

//...생략...

Timer.periodic(Duration(milliseconds: 10), (timer) {
  // 할 일
}
```

---

## 3. 코드 해석

+ **반복 실행 주기를 `Timer.periodic()` 메서드의 첫 번째 인수 `Duration 인스턴스에 설정`하면 `두 번째 인수로 받은 함수에서 실행되는 구조`입니다.**
+ `Duration 클래스는 주기를 정의`합니다. 
+ Duration 클래스의 생성자에 지정할 수 있는 프로퍼티는 다음과 같습니다.
    - `days`: 날짜
    - `hours`: 시간
    - `minutes`: 분
    - `seconds`: 초
    - `milliseconds`: 천 분의 1초
    - `microseconds`: 백만 분의 1초


+ 예를 들어 다음 코드는 1시간 30분마다 한 번씩 실행됩니다.

```dart
Duration(hours: 1, minutes: 30, (timer) {
  // 동작
})
```

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)
