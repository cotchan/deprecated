---
title: Flutter) 키보드 숨기기
author: cotchan
date: 2021-06-02 15:36:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 키보드 숨기는 방법

+ 키보드를 사라지게 하고 싶은 타이밍(onTap, onPress 등)에 아래 코드를 적용하면 됩니다.

```dart
FocusScopeNode currentFocus = FocusScope.of(context);
currentFocus.unfocus();
```

---

+ **추가 조치로 `GestureDetector`의 `onTap`을 통해 아무 일도 안 일어나도록 할 수 있습니다.**

```dart
@override
Widget build(BuildContext context) {
  final FocusScopeNode currentFocus = FocusScope.of(context);
  currentFocus.unfocus();

  return GestureDetector(
    onTap: () {
    },
    child: Container()
  );
}
```

---

+ 출처
  + [FLUTTER 키보드 숨기기](https://devghyun.github.io/2019/11/21/flutter%20%ED%82%A4%EB%B3%B4%EB%93%9C%20%EC%88%A8%EA%B8%B0%EA%B8%B0/)

