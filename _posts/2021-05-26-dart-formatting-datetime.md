---
title: dart) DateTime Formatting in dart(DateTime을 원하는 형식의 String으로 변환)
author: cotchan
date: 2021-05-26 20:51:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. Sample 1

```dart
import 'package:intl/intl.dart';

final now = DateTime.now();
String dateFormatted = DateFormat('yyyy-MM-dd').format(now);
```

---

## 2. Sample 2

+ **분까지 나타내는 경우**

```dart
import 'package:intl/intl.dart';

//final DateTime item.submittedTime;

Text(DateFormat('yyyy-MM-dd hh:mm').format(item.submittedTime),
  textAlign: TextAlign.center,
  style: const TextStyle(color: Color(0xff868F9B), fontSize: 12, fontFamily: 'NotoSansKR'))
```

---

+ **초까지 나타내는 경우**

```dart
import 'package:intl/intl.dart';

//final DateTime item.submittedTime;

Text(DateFormat('yyyy-MM-dd hh:mm:ss').format(item.submittedTime),
  textAlign: TextAlign.center,
  style: const TextStyle(color: Color(0xff868F9B), fontSize: 12, fontFamily: 'NotoSansKR'))
```

---

+ 출처
  + [How to format DateTime in Flutter? Remove milliseconds in DateTime?](https://stackoverflow.com/questions/66349547/how-to-format-datetime-in-flutter-remove-milliseconds-in-datetime)

