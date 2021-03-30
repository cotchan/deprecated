---
title: Flutter) [ui] 7.1 UI 뼈대 작성(initialization)
author: cotchan
date: 2021-03-30 19:07:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 핵심 구성 요소

- **`BottomNavigationBar`**
  - 하단의 탭을 구성합니다.
- **`AppBar`**
  - 상단의 제목줄을 구성합니다.
- **`Row, Column`**
  - 가로, 세로로 레이아웃을 구성합니다.
- **`GestureDetector`**
  - 어떤 위젯이라도 클릭 가능하게 해줍니다.
- **`Opacity`**
  - 투명도를 부여합니다.
- **`CarouselSlider`**
  - 좌우로 슬라이드되는 UI 작성시 사용하는 별도의 라이브러리

---

## 2. UI 뼈대 작성

+ `main.dart`

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

```

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

