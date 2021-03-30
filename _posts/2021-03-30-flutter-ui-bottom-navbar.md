---
title: Flutter) [ui] 7.2 하단 탭 구성(BottomNavigationBar)
author: cotchan
date: 2021-03-30 00:00:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. BottomNavigationBar 이란 

- 머티리얼 디자인 앱의 뼈대인 `Scaffold` 위젯은 **상단 제목이 표시**되는 `AppBar` 위젯뿐만 아니라 **하단 탭을 구성하는 `BottomNavigationBar` 위젯**도 지원합니다.

- **하단에 탭을 배치하려면 `Scaffold 위젯의 bottomNavigationBar 프로퍼티에 BottomNavigationBar 인스턴스를 정의`하면 됩니다.**

---

## 2. 하단 탭 작성

+ `before`

```dart
class _MyHomePageState extends State<MyHomePage> {

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

---

+ **`after`**

```dart
//하단 탭 작성
class _MyHomePageState extends State<MyHomePage> {
  var _index = 0; // 1111. 페이지 인덱스 0, 1, 2

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(  // 22222
      appBar: AppBar(
        title: Text('복잡한 UI'),
      ),
      body: Center(
        child: Text(
          '$_index 페이지',
          style: TextStyle(fontSize: 40),
        ),
      ),
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
```

---

## 2-1. after 코드 해석

+ 11111
  + 3개의 페이지를 표현할 수 있도록 `_index` 변수를 준비합니다. 각 페이지는 0,1,2 값을 가지게 합니다.
+ 22222
  + build() 메서드에 작성된 Container() 코드를 Scaffold 위젯으로 수정했습니다.
  + appBar 프로퍼티와 body 프로퍼티 작성은 지금까지와 비슷합니다.
+ 33333
  + bottomNavigationBar 프로퍼티에 탭을 3개 작성합니다.
+ 44444
  + 탭을 클릭하면 `onTap` 이벤트가 동작하고 선택된 탭의 인덱스가 전달됩니다.
  + 또한 `currentIndex`에 설정된 `_index` 값을 새로 클릭한 탭의 인덱스로 교체한 후 `setState` 메서드를 사용해 화면을 갱신합니다.
+ 55555
  + `currentIndex` 프로퍼티에는 현재 선택된 탭이 어떤 것인지 0번부터 시작하는 인덱스 번호를 지정해줍니다.
+ 66666
  + `items` 프로퍼티에는 BottomNavigationBarItem 위젯의 리스트를 정의합니다.
  + **이 위젯은 탭을 정의하고 `title`과 `icon을 지정`할 수 있습니다.**


---

## 2-2. Sample Code

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
  var _index = 0; // 1111. 페이지 인덱스 0, 1, 2

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(  // 22222
      appBar: AppBar(
        title: Text('복잡한 UI'),
      ),
      body: Center(
        child: Text(
          '$_index 페이지',
          style: TextStyle(fontSize: 40),
        ),
      ),
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
```

---

## 2-3. 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-41.png)




---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

