---
title: Flutter) [ui] 7.3 상단 바 변경 방법(AppBar)
author: cotchan
date: 2021-03-30 19:27:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. AppBar 란?

- **`AppBar 위젯은 제목이나 메뉴를 표시`하는 등 머티리얼 디자인에서 앱의 통일성을 위해 꼭 필요한 요소입니다.**
- AppBar 위젯에는 여러 가지 프로퍼티가 있어서 다양하게 수정 가능합니다.

---

## 2. 제목의 가운데 정렬과 색상 변경

+ Before

```dart
//before
return Scaffold( 
      appBar: AppBar(
        title: Text('복잡한 UI'),
      ),
```

---

+ **`After`**
  + 제목이 가운데로 이동하고 색상이 변경됩니다.


```dart
return Scaffold(
  appBar: AppBar(
    backgroundColor: Colors.white,  // 11111. 배경색을 흰색으로
    title: Text(
      '복잡한 UI',
      style: TextStyle(color: Colors.black),  // 22222. 글자색을 검은색으로
    ),
    centerTitle: true,  // 33333. 제목을 가운데
  ),
```

---

## 2-1. 코드 해석

+ 11111
  + `backgroundColor` 프로퍼티에 흰색을 지정하여 AppBar 위젯의 배경색을 변경합니다.
+ 22222
  + 기본 테마의 제목은 흰색이 기본값이기 때문에 글자를 검은색으로 변경해서 홤녀에 잘 보이도록 합니다.
  + `Colors` 클래스에는 다양한 색상값이 미리 상수로 정의되어 있어서 간단하게 색상을 선택할 때 유용합니다.
+ 33333
  + **안드로이드에서는 기본적으로 제목이 좌측으로 정렬됩니다.**
  + `centerTitle` 프로퍼티를 지정하면 제목을 가운데에 표시할 수 있습니다.

---

## 2-2. 결과 화면

+ Before

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-42.png)

---

+ After
  + 제목이 가운데로 이동하고 색상이 변경됩니다.

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-43.png)

---

## 3. AppBar 오른쪽에 메뉴 추가

- **AppBar 위젯의 `actions 프로퍼티에 위젯의 리스트를 정의`하여 간단히 메뉴를 추가할 수 있습니다.**
- AppBar 위젯의 actions 프로퍼티를 다음과 같이 작성합니다.

```dart
appBar: AppBar(
  title: Text('복잡한 UI'),
  actions: <Widget>[
    IconButton(
      icon: Icon(Icons.add,
    color: Colors.black,
    ),
    onPressed: (){}
    ),
  ],
),
```

---

- AppBar 위젯의 actions 프로퍼티에는 어떠한 위젯도 리스트로 배치할 수 있습니다.
- **일반적으로 AppBar 메뉴를 작성할 때는 `IconButton 위젯을 사용`합니다.**
  - IconButton 위젯은 `icon과 onPressed 프로퍼티를 정의`할 수 있습니다.

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-44.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

