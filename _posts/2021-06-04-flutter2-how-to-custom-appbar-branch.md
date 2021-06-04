---
title: Flutter) AppBar BackButton 등장 여부 분기 방법(AppBar 커스텀 시)
author: cotchan
date: 2021-06-04 09:47:00 +0800
categories: [Flutter]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Intro

+ **AppBar 커스텀 시 BackButton 등장 여부를 판단할 때 `pop 가능 여부를 판단해야합니다.`**

---

## 2. 화면 pop 가능여부 판단

+ **`ModalRoute.of(context)`을 통해 pop 가능여부를 판단합니다.**

```dart
AppBar CustomAppBar(BuildContext) {
  final ModalRoute<dynamic>? parentRoute = ModalRoute.of(context);
  if (parentRoute?.canPop ?? false) { 
    return AppBar(
      leading: IconButton(
        icon: const Icon(Icons.arrow_back),
        onPressed: () => Navigator.of(context).pop(),
      ),
      //...
    );
  } 
  else {
    return AppBar(
      //...
    );
  }
}
```

---

## 3. ModalRoute<T> class

+ ModalRoutes cover the entire Navigator.

+ **`of` (Static Methods)**
  + Returns the modal route most closely associated with the given context.

---

+ 출처
  + [ModalRoute<T> class](https://api.flutter.dev/flutter/widgets/ModalRoute-class.html)
