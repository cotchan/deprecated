---
title: Flutter) [ui] 7.9 하단 부분(글 목록 리스트 뿌리기, ListView, ListTile)
author: cotchan
date: 2021-03-30 20:30:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. ListView, ListTile 사용하기

- 하단에는 공지사항 같은 느낌으로 `글 목록을 표시`할 것입니다.
- ListView 위젯과 ListTile 위젯을 활용하면 간단히 구현할 수 있습니다.
- `_buildBottom() 메서드`를 다음과 같이 수정합니다.
- **스크롤 안에 스크롤 객체를 넣을 때는 `shrinkWrap`과 `physics` 프로퍼티를 정의하는 것에 유의해야 합니다.**

```dart
//하단
Widget _buildBottom() {   
  final items = List.generate(10, (i) {   // 11111.
    return ListTile(
      leading: Icon(Icons.notifications_none),
      title: Text('[이벤트] 이것은 공지사항입니다'),
    );
  });
  return ListView(
    physics: NeverScrollableScrollPhysics(),  //44444. 이 리스트의 스크롤 동작 금지
    shrinkWrap: true,   // 33333. 이 리스트가 다른 스크롤 객체 안에 있다면 true로 설정해야 함
    children: items,    // 22222.
  );
}
```

---

## 2. 코드 해석

+ 11111
  + List.generate() 함수는 0부터 9까지의 수(10개)를 생성하여 두 번째 인수의 함수에 i 매개변수로 전달합니다.
  + i 값을 전달받아 `ListTile` 위젯 형태로 변환하여 그것들의 리스트가 반환됩니다.
+ 22222
  + 임시글 10개를 작성한 후 `ListView` 위젯의 `children 프로퍼티`에 설정했습니다.
+ 33333
  + 평소라면 children 프로퍼티만 설정하면 됩니다.
  + 하지만 이 예제처럼 스크롤 가능한 객체 안에 다시 스크롤 객체를 넣는 경우에는 **`shrinkWrap` 프로퍼티를 `true`로 설정해줘야 합니다.**
  + **`ListView` 위젯 안에 `ListView` 위젯이 들어간 상황**
+ 44444
  + 이 상태로 실행하면 하단 부분에서는 스크롤 동작이 안 되는 현상이 발생합니다.
  + 이는 스크롤 안에 스크롤을 넣은 경우로 안쪽 스크롤을 막아서 정상 동작이 되도록 `physics` 프로퍼티에 `NeverScrollableScrollPhysics` 클래스의 인스턴스를 설정했습니다.
  + **이것으로 이 리스트는 스크롤 기능이 정지되어 바깥쪽 스크롤이 정상적으로 동작하게 됩니다.**  

---

## 3. Sample Code

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
    return ListView(
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

//중단
Widget _buildMiddle() {   //22222
  return Text('Middle');
}

//하단
Widget _buildBottom() {
  final items = List.generate(10, (i) {   // 11111.
    return ListTile(
      leading: Icon(Icons.notifications_none),
      title: Text('[이벤트] 이것은 공지사항입니다'),
    );
  });
  return ListView(
    physics: NeverScrollableScrollPhysics(),  //44444. 이 리스트의 스크롤 동작 금지
    shrinkWrap: true,   // 33333. 이 리스트가 다른 스크롤 객체 안에 있다면 true로 설정해야 함
    children: items,    // 22222.
  );
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

## 4. 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-50.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

