---
title: dart) Timestamp 변환방법(DateTime => Int)
author: cotchan
date: 2021-05-26 20:06:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

+ **`.millisecondsSinceEpoch`**

```dart
void main() {
  print(DateTime.now().millisecondsSinceEpoch);
}
```

---

+ 출처
  + [How to get a timestamp in Dart?](https://stackoverflow.com/questions/13110542/how-to-get-a-timestamp-in-dart)
  + [DateTime class](https://api.dart.dev/stable/2.10.1/dart-core/DateTime-class.html)

