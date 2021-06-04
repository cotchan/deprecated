---
title: Flutter) 체크 박스 터치 영역 변경
author: cotchan
date: 2021-06-04 13:38:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. issue

+ 체크 박스로 `선택`/`미선택`을 구분했었는데 체크 박스가 잘 선택이 안 되는 문제가 발생했습니다.
+ **그래서 해당 줄 전체를 `터치영역으로 조정`하여 해결하였습니다.**

---

## 2. BEFORE

+ 아래와 같이 CheckBox를 사용하여 선택/미선택을 표현하였습니다.

```dart
//before
bool updateRowState(int idx);


Widget _drawSelectButton(int idx) {
  return CustomCheckBox(
    checked: viewModel.isSelected(idx),
    onChanged: (value) {
      viewModel.updateRowState(idx, value);
    },
    checkedLabelStyle: theme.textTheme.label2,
  );
}

child: ListTile(
  title: Row(
    children: <Widget>[
      _drawSelectButton(idx),
      const SizedBox(width: 10),
      Flexible(
        child: Column(
        //...
```

---

## 3. After

+ **`onTap 프로퍼티를 추가`하여 해당 줄 전체를 터치영역으로 조정**

```dart
//after
bool updateRowState(int idx)


Widget _drawSelectButton(int idx) {
  return FMCheckBox(
    checked: viewModel.isSelected(idx),
    onChanged: (value) {
    },
    checkedLabelStyle: theme.textTheme.label2,
  );
}


child: ListTile(
  onTap: () {
    viewModel.updateRowState(idx);
  },
  title: Row(
    children: <Widget>[
      _drawSelectButton(idx),
      const SizedBox(width: 10),
      Flexible(
        child: Column(
        //...
```

---

+ 출처

