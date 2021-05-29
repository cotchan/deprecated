---
title: Flutter) 스크롤 화면 만드는 법(ex. 인쇄화면 새로고침)
author: cotchan
date: 2021-05-26 21:08:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Sample1

+ **`PageView` 사용**

```dart
Widget _drawPrintListBody() {
  return NotificationListener<OverscrollIndicatorNotification>(
    onNotification: (overScroll) {
      overScroll.disallowGlow();
      return false;
    },
    child: PageView(
      scrollDirection: Axis.vertical,
      physics: const AlwaysScrollableScrollPhysics(),
      children: [
        Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Resource.images.print.empty.image(width: 156, height: 156),
            const SizedBox(height: 16),
            Text(Resource.messages.printPageLabelEmptyList,
                textAlign: TextAlign.center,
                style: const TextStyle(
                    color: Color(0xFF868F9B),
                    fontSize: 12,
                    fontFamily: 'NotoSansKR')),
          ],
        )
      ],
    ),
  );
  //...
}
```

---

## 2. Sample2

+ **`SingleChildScrollView` 사용**

```dart
Widget build(BuildContext context) {
  final theme = FMTheme.of(context);
  return Align(
    child: SingleChildScrollView(
      physics: const AlwaysScrollableScrollPhysics(),
      clipBehavior: Clip.none,
      child: Column(
        children: [
          ConstrainedBox(
            constraints: const BoxConstraints(maxWidth: 220, maxHeight: 134),
            child: Image(image: image, fit: BoxFit.fitWidth),
          ),
          const SizedBox(height: 30),
          Text(title, style: theme.textTheme.emptyTitle),
          const SizedBox(height: 10),
          Text(message, style: theme.textTheme.emptySubTitle),
        ],
      )
    )
  );
}
```

---

+ 출처
  + [SingleChildScrollView 위 / 아래 스크롤 효과 없애기](https://velog.io/@gunwng123/SingleChildScrollView-%EC%9C%84-%EC%95%84%EB%9E%98-%ED%9A%A8%EA%B3%BC-%EC%97%86%EC%95%A0%EA%B8%B0)

