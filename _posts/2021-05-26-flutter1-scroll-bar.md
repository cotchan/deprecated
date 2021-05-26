---
title: Flutter) 스크롤바 추가하는 방법
author: cotchan
date: 2021-05-26 21:00:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 스크롤바 추가하는 법

+ **`Scrollbar` 위젯으로 감싸주면 됩니다.**

```dart
Widget _drawPrintListBody() {
  return Scrollbar(
    child: ListView.separated(
      itemCount: viewModel.printList.length + 2,
      separatorBuilder: (context, index) => FMDivider.list(),
      itemBuilder: (context, index) {
        if (index == 0 || index == len + 1) {
          return Container();
        }
        return _drawListItem(printItems[index - 1], index - 1);
      },
    ),
  //...
}
```

---

+ 출처

