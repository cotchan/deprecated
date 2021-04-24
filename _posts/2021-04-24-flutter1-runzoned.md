---
title: Flutter) runZoned
author: cotchan
date: 2021-04-24 16:04:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. zone이란?

- **Zone은 실행되는 공간을 나눠서, 그 공간 안에 있는 한 안전하게 프로그램이 돌아가도록 합니다.**
- **zone은 프로그램이 예상치 못한 에러로부터 종료하는 걸 막을 때 쓰입니다.**
- 일반적인 상황이면 프로그램이 종료되는 경우더라도 zone(존)을 쓰면 잘 돌아가게 되는 것입니다.

+ 예시 코드

```dart
//Before
void main() {
  setupLocator();
  runApp(MyApp());
}

//After
void main() {
  setupLocator();
  runZoned(() {
    runApp(MyApp());
  }, onError: );
}

```

---

+ 출처
  + [Flutter - Zone이란? 프로그램 종료되지 않게 예외처리 하기.](https://software-creator.tistory.com/19)
