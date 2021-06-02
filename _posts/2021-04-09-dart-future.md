---
title: dart) Future, Async execution
author: cotchan
date: 2021-04-09 22:12:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. dart의 비동기 메서드 처리

+ **일반적으로 Dart에서 수행하는 모든 작업은 `UI-Thread`에서 시작됩니다.**
+ **sync, async를 사용하든 Dart에서 호출하는 방법이 무엇이든 `Dart는 단일 스레드`이기 때문에 UI-Thread에서 실행**
+ **Javascript 및 Dart와 같은 단일 스레드 언어에서 `비동기 메서드는 병렬로 실행되지 않고 이벤트 루프에 의해 처리되는 규칙적인 이벤트 시퀀스를 따릅니다.`**

---

+ **Dart는 단일 스레드이기 때문에 Future 결과를 얻는 데 사용되는 `비동기 메서드는 메인 스레드에서 실행됩니다.`**
+ **새 스레드에서 함수를 실행하고 싶다면 `isolate`를 만들어서 할 수 있습니다.**
  + 이에 대해 Dart는 `compute`라는 사용하기 쉬운 `isolate wrapper`를 제공합니다. 
  + 이 wrapper에, 처리할 하나의 메서드와 처리할 데이터만 전달하면 나중에 모든 결과를 반환받을 수 있습니다.

---

+ 출처
  + [I am having a problem with Flutter/Dart Async code execution, as in how does it work](https://stackoverflow.com/questions/61033892/i-am-having-a-problem-with-flutter-dart-async-code-execution-as-in-how-does-it)
  + [Dart 언어 Future 알아보기](https://beomseok95.tistory.com/309)
  + [Asynchronous programming: futures, async, await](https://dart.dev/codelabs/async-await)
  + [다트 future, async, await](https://brunch.co.kr/@mystoryg/134)
  + [Future class](https://api.dart.dev/stable/2.10.5/dart-async/Future-class.html#constructors)
