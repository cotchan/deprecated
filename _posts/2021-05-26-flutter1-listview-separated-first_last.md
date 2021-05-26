---
title: Flutter) ListView.separated 시작점, 끝점에 라인 생성하는 법
author: cotchan
date: 2021-05-26 20:25:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## Sample 1

```dart
ListView.separated(
      itemCount: user.personList.length + 2,
      separatorBuilder: (context, index) => Divider(),
      itemBuilder: (context, index) {
        if (index == 0 || index == user.personList.length + 1) {
          return Divider(
            color: Colors.black,
            thickness: 2,
          );
        }
        return ListTile(title: Text((index - 1).toString())); // note: you have to access element by -1;
      },
    ),
```

---

## Sample 2

```dart
Widget _drawPrintListBody() {
  return ListView.separated(
    itemCount: viewModel.printList.length + 2,
    separatorBuilder: (context, index) => FMDivider.list(),
    itemBuilder: (context, index) {
      if (index == 0 || index == len + 1) {
        return Container();
      }
      return _drawListItem(printItems[index - 1], index - 1);
    },
  );
}
```

---

+ 출처
  + [Flutter - How to add first top and last bottom list with divider() on ListView.separated()](https://stackoverflow.com/questions/60826670/flutter-how-to-add-first-top-and-last-bottom-list-with-divider-on-listview-s)

