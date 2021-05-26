---
title: Flutter) ListTile에 배경색 넣기(background color of ListTile)
author: cotchan
date: 2021-05-26 20:35:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. SampleCode 1

```dart
ListView (
    children: <Widget>[
        new Container (
            decoration: new BoxDecoration (
                color: Colors.red
            ),
            child: new ListTile (
                leading: const Icon(Icons.euro_symbol),
                title: Text('250,00')
            )
        )
    ]
)
```

---

## 2. SampleCode 2

```dart
Widget _drawListItem(PrintItem item, int idx) {
  return Container(
    decoration: BoxDecoration(
        color: viewModel.isSelected(idx)
            ? const Color(0xFFF2F5F9)
            : Colors.white),
    child: ListTile(
      dense: true,
      contentPadding:
          const EdgeInsets.symmetric(vertical: 13, horizontal: 20),
      title: Row(
  //...
}
```

---

+ 출처
  + [Change background color of ListTile upon selection in Flutter](https://stackoverflow.com/questions/49331612/change-background-color-of-listtile-upon-selection-in-flutter)

