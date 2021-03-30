---
title: Flutter) [ui] 7.7 상단 부분(완성중)
author: cotchan
date: 2021-03-30 20:04:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Overview

+ Page1을 다음과 같이 수정합니다.
  + 상단, 중단, 하단 부분을 각 각 메서드로 분리헀습니다.
  + 그리고 Page1의 `build()` 메서드에는 `Column`으로 세 영역을 배치했습니다.

```dart
class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        _buildTop(),
        _buildMiddle(),
        _buildBottom(),
      ],
    );
  }
}

//상단
Widget _buildTop() {    //11111
  return Text('Top');
}

//중단
Widget _buildMiddle() {   //22222
  return Text('Middle');
}

//하단
Widget _buildBottom() {   //33333
  return Text('Bottom');
}
```

---

## 2. 택시 Icon 메뉴 한 개 만들기

+ **상단 부분에 2행 4열짜리 메뉴를 구성한다고 합시다.**
+ 각 메뉴는 다시 상단에는 이미지, 하단에는 글자를 표시하는 2행짜리 아이템입니다.

- 먼저 메뉴 하나를 작성하기 위해 `_buildTop()` 메서드를 다음과 같이 수정합니다.
  - `Column` 위젯 안에 택시 아이콘과 글자를 수직으로 배치합니다.

```dart
//상단
Widget _buildTop() {    //11111
  return Column(
    children: <Widget>[
      Icon(
        Icons.local_taxi,
        size: 40,
      ),
      Text('택시'),
    ],
  );
}
```

---

+ Sample Code

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

//하단 탭 작성
class _MyHomePageState extends State<MyHomePage> {
  var _index = 0;
  var _pages = [  //11111
    Page1(),
    Page2(),
    Page3(),
  ];

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
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
      body: _pages[_index],
      bottomNavigationBar: BottomNavigationBar( // 33333.
        onTap: (index) { // 44444.
          setState(() {
            _index = index; //선택된 탭의 인덱스로 _index 값을 변경
          });
        },
        currentIndex: _index, // 55555. 선택된 인덱스
        items: <BottomNavigationBarItem>[ // 66666.
          BottomNavigationBarItem(
            title: Text('홈'),
            icon: Icon(Icons.home),
          ),
          BottomNavigationBarItem(
            title: Text('이용서비스'),
            icon: Icon(Icons.assignment),
          ),
          BottomNavigationBarItem(
            title: Text('내 정보'),
            icon: Icon(Icons.account_circle),
          ),
        ],
      ),
    );
  }
}

class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        _buildTop(),
        _buildMiddle(),
        _buildBottom(),
      ],
    );
  }
}

//상단
Widget _buildTop() {    //11111
  return Column(
    children: <Widget>[
      Icon(
        Icons.local_taxi,
        size: 40,
      ),
      Text('택시'),
    ],
  );
}

//중단
Widget _buildMiddle() {   //22222
  return Text('Middle');
}

//단
Widget _buildBottom() {   //33333
  return Text('Bottom');
}

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '이용서비스',
        style: TextStyle(fontSize: 40),
      ),
    );
  }
}

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '내정보',
        style: TextStyle(fontSize: 40),
      ),
    );
  }
}
```

---

+ 수정 시 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-48.png)

---

## 3. 메뉴를 한 줄에 4개 만들기

- **Column을 Row로 감싸고 Column을 복사&붙여넣기로 3개 더 나열하여 다음과 같은 형태가 되도록 합니다.**
  - **Column에 커서를 놓고 단축키 option + Enter 누른 후 Wrap with Row 기능 활용**
  - Text는 적당한 값으로 수정합니다.

+ **수정한 `_buildTop()`**

```dart
//상단
Widget _buildTop() {    //11111
  return Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: <Widget>[
      Column(
        children: <Widget>[
          Icon(
            Icons.local_taxi,
            size: 40,
          ),
          Text('택시'),
        ],
      ),
      Column(
        children: <Widget>[
          Icon(
            Icons.local_taxi,
            size: 40,
          ),
          Text('블랙'),
        ],
      ),
      Column(
        children: <Widget>[
          Icon(
            Icons.local_taxi,
            size: 40,
          ),
          Text('바이크'),
        ],
      ),
      Column(
        children: <Widget>[
          Icon(
            Icons.local_taxi,
            size: 40,
          ),
          Text('대리'),
        ],
      ),
    ],
  );
}
```

+ Row는 기본적으로 아이템을 왼쪽부터 배치합니다.
+ 가로를 꽉 채우면서 적당한 간격을 유지하기 위해 `mainAxisAlignment` 프로퍼티에 위젯 사이의 공간을 동일한 비율로 정렬하는 의미의 `spaceEvenly`를 설정했습니다.

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-49.png)

---

## 4. 메뉴를 두 줄로 만들기





---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

