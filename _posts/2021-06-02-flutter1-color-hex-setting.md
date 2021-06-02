---
title: Flutter) Hex값을 사용해 색상 설정하는 방법
author: cotchan
date: 2021-06-02 15:43:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Colors에 RGB값 설정방법

+ **불투명도를 지정해주는 `0xff뒤에` Hex값을 붙이면 됩니다.**

```dart
//example
backgroundColor: const Color(0xFFEAEEF2),
disabledBackgroundColor: const Color(0xFFEAEEF2),
highlightBackgroundColor: const Color(0xFFD1D9E2),
```

---

+ 출처
  + [[Flutter] Flutter에서 Hex값을 사용해 색상 설정 방법](https://bigstar-vlog.tistory.com/68)
