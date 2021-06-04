---
title: Flutter) 버튼 클릭 시 나타나는 하이라이트 크기 조절방법
author: cotchan
date: 2021-06-04 10:03:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. splashRadius

+ **버튼 클릭 시 나타나는 하이라이트 크기 조절은 `IconButton`의 `splashRadius` 크기 조절을 통해 가능합니다.**

```dart
child: IconButton(
  splashRadius: 20,
  //... 기타 속성
),
```

---

+ 출처

