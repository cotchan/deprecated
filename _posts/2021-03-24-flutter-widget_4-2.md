---
title: Flutter) [위젯] 4.2 화면 배치에 쓰는 기본 위젯
author: cotchan
date: 2021-03-24 18:46:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ **화면 배치에 사용되는 기본 위젯입니다.**

---

## 1. Container

+ 아무것도 없는 위젯입니다.
+ `가로`와 `세로` 길이, `색`, `안쪽 여백(padding)`, `바깥쪽 여백(margin)` 등의 설정이 가능합니다.
+ **`child 프로퍼티`로 또 다른 위젯을 자식으로 가질 수 있습니다.**

+ Sample Code

```dart
//main.dart

import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: Container(
        color: Colors.red,
        width: 100,
        height: 100,
        padding: const EdgeInsets.all(8.0),
        margin: const EdgeInsets.all(8.0)
      )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-01.png)

---

## 2. Column

+ **`수직 방향`으로 위젯들을 나란히 배치하는 위젯입니다.**
+ 레이아웃의 대부분은 `Column`과, `Row`를 조합하여 만들기 때문에 매우 자주 사용됩니다.
+ **`children 프로퍼티`에는 여러 위젯의 리스트를 지정할 수 있습니다. 지정한 위젯들은 `세로로 배치`됩니다.**

```dart
Column(
  children: <Widget>[
    [위젯],
    [위젯],
    [위젯],
  ],
),
```

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: Column(
        children: <Widget>[
          Container(
            color: Colors.red,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.green,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.blue,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
        ],
      )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-02.png)

---

## 3. Row

+ **Column과 반대로 `수평 방향`으로 위젯들을 나란히 배치하는 위젯입니다.**
+ 레이아웃의 대부분은 Column과 Row를 조합하여 만들기 때문에 매우 자주 사용됩니다.
+ **Column과 같이 `children 프로퍼티`에 여러 위젯을 나열합니다.**



+ Row, Column과 같이 방향성이 있는 위젯은 mainAxis와 crossAxis 관련 프로퍼티를 사용합니다.


```dart
Row(
  mainAxisSize: MainAxisSize.max,  //가로로 꽉 채우기
  mainAxisAlignment: MainAxisAlignment.center,  //가로 방향으로 가운데 정렬하기
  crossAxisAlignment: CrossAxisAlignment.center, //세로 방향으로 가운데 정렬하기
  children: <Widget>[
    [위젯],
    [위젯],
    [위젯],
  ],
),
```

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: Row(
        mainAxisSize: MainAxisSize.max,
        mainAxisAlignment: MainAxisAlignment.center,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Container(
            color: Colors.red,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.green,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.blue,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
      ],
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-03.png)


---

## 3-1. mainAxis

## 3-2. crossAxis

## 3-3. MainAxisSize

## 3-4. MainAxisAlignment

## 3-5. CrossAxisAlignment

---

## 4. Stack

+ **Stack 위젯은 children에 나열한 여러 위젯을 `순서대로 겹치게 합니다.`**
+ Stack을 사용하는 경우
  + 사진 위에 글자를 표현할 때 사용
  + 화면 위에 로딩 표시를 하는 상황에 사용
+ 사용 방법은 `Row`, `Column`과 동일합니다.
+ **적용 순서**
  + children 프로퍼티에 정의한 리스트에 `먼저 작성한 위젯이 가장 아래쪽에 위치`
  + children 프로퍼티에 정의한 리스트에 `나중에 작성한 위젯이 가장 위쪽에 위치`

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: Stack(
        children: <Widget>[
          Container(
            color: Colors.red,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.green,
            width: 80,
            height: 80,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.blue,
            width: 60,
            height: 60,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
      ],
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-04.png)


---

## 5. SingleChildScrollView

+ **Column을 사용하여 위젯들을 나열하다가 `화면 크기를 넘어서면 스크롤`이 필요합니다.**
+ 그럴 때는 SingleChildScrollView로 감싸서 스크롤이 가능하게 할 수 있습니다.
+ SingleChildScrollView는 말 그대로 하나의 자식을 포함하는 스크롤이 가능한 위젯입니다.
+ SingleChildScrollView는 `하나의 자식 위젯을 가져야` 하기 때문에 Column을 사용하여 상하 스크롤을 구현할 수 있습니다.
  + Column은 기본적으로 표시할 위젯의 크기만큼 가로 길이를 가집니다.
  + `ListBody`를 사용하면 스크롤 가능 영역이 가로로 꽉 차기 때문에 사용자가 스크롤하기 더 쉽습니다.

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  final items = List.generate(100, ((i) => i)).toList();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: SingleChildScrollView(
        child: ListBody(
          children: items.map((i) => Text('$i')).toList(),
        ),
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-05.png)

---

## 6. ListView, ListTile

+ **ListView는 리스트를 표시하는 위젯입니다.**
+ SingleChildScrollView와 ListBody의 조합과 동일한 효과를 내지만 좀 더 리스트 표현에 최적화된 위젯
+ ListView에 표시할 각 항목의 레이아웃은 직접 정의해도 되지만 리스트 아이템을 쉽게 작성할 수 있는 `ListTile` 위젯을 사용하면 편리합니다.
+ ListView의 children 프로퍼티에 다수의 위젯을 배치하면 정적인 리스트를 쉽게 만들 수 있습니다.


+ **ListTile 위젯의 leading, title, traling 프로퍼티는 아래의 의미를 가집니다.**
  - **`leading` - 왼쪽 위치를 담당**
  - **`title` - 중앙 위치를 담당**
  - **`trailing` - 오른쪽 위치를 담당**
  - 이를 활용해 자유롭게 아이콘이나 글자를 배치할 수 있습니다.

+ **ListTile의 `onTap` 프로퍼티에는 리스트의 항목을 탭(터치)했을 때 실행해야 하는 동작을 정의한 함수를 작성합니다.**

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: ListView(
        scrollDirection: Axis.vertical,
        children: <Widget>[
          ListTile(
            leading: Icon(Icons.home),
            title: Text('Home'),
            trailing: Icon(Icons.navigate_next),
            onTap: () {},
          ),
          ListTile(
            leading: Icon(Icons.home),
            title: Text('Event'),
            trailing: Icon(Icons.navigate_next),
            onTap: () {},
          ),
          ListTile(
            leading: Icon(Icons.home),
            title: Text('Camera'),
            trailing: Icon(Icons.navigate_next),
            onTap: () {},
          ),
        ],
      )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-06.png)


---

## 7. GridView

+ 열 수를 지정하여 그리드 형태로 표시하는 위젯입니다.
+ **`GridView.count() 생성자`는 간단하게 그리드를 작성하게 해줍니다.**
+ `crossAxisCount` 프로퍼티에 열 수를 지정할 수 있습니다.

```dart
GridView.count(
  crossAxisCount: [열 수],
  children: <Widget>[
    [위젯],
    [위젯],
    [위젯],	
  ],
),
```

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: GridView.count(
        crossAxisCount: 2,  //열 수
        children: <Widget>[
          Container(
            color: Colors.red,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.green,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
          Container(
            color: Colors.blue,
            width: 100,
            height: 100,
            padding: const EdgeInsets.all(8.0),
            margin: const EdgeInsets.all(8.0),
          ),
        ],
      )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-07.png)

---

## 8. PageView

+ **여러 페이지를 좌우로 슬라이드하여 넘길 수 있도록 해주는 위젯입니다.**
+ children 프로퍼티에 각 화면을 표현할 위젯을 여러 개 준비하여 지정하면, 화면을 좌우로 슬라이드 할 수 있습니다.
+ 하지만 Tab과 연동하여 사용하지 않으면 좌우로 슬라이드가 가능한지 사용자가 모를 수 있어서 단독으로는 잘 사용하지 않습니다.

```dart
PageView(
  children: <Widget>[
    [위젯],
    [위젯],
    [위젯],
  ],
),
```

---

+ Sample Code
  + 배치되는 위젯은 하나의 페이지를 담당합니다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('제목'),
      ),
      body: PageView(
        children: <Widget>[
          Container(
            color: Colors.red,
          ),
          Container(
            color: Colors.green,
          ),
          Container(
            color: Colors.blue,
          ),
        ]
      )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-08.png)

---

## 9. AppBar, TabBar, Tab, TabBarView

+ 이 위젯들을 조합하여 PageView와 유사하지만 페이지와 탭이 연동되는 화면을 구성할 수 있습니다.
+ **`탭이 있기 때문에` PageView만 단독으로 사용하는 것보다 사용성이 높습니다.**

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(  //Scaffold를 감싸고
        length: 3,  //탭 수 지정
        child: Scaffold(
          appBar: AppBar(
            title: Text('Tab'),
            bottom: TabBar( //Scaffold의 bottom 프로퍼티에 TabBar 지정
              tabs: <Widget>[ //tabs 프로퍼티에 Tab의 리스트 지정
                Tab(icon: Icon(Icons.tag_faces)), //Tab에는 아이콘이나 글자를 표시할 수 있습니다.
                Tab(text: '메뉴2'),
                Tab(icon: Icon(Icons.info), text: '메뉴3'),
              ],
            ),
          ),
          body: TabBarView( //Scaffold의 body 프로퍼티에는 TabBarView 배치
            children: <Widget>[ //children 프로퍼티에 표시할 화면 배치
              Container(color: Colors.yellow,),
              Container(color: Colors.orange,),
              Container(color: Colors.red,),
            ]),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-09.png)

---

## 10. BottomNavigationBar

+ **`하단에 2~5개의 탭 메뉴`를 구성할 수 있는 위젯입니다.**
+ 각 탭을 클릭하여 화면을 전환할 때 사용합니다.
+ **사용 방법**
  1. Scaffold의 프로퍼티 중에서 `bottomNavigationBar 프로퍼티를 정의`하고 
  2. items 프로퍼티에 `BottomNavigationBarItem 위젯들을 나열`합니다.
  3. `icon과 title 프로퍼티를 정의`하여 간단히 하단 탭 바를 구성할 수 있습니다.

---

+ Sample Code
  + body 프로퍼티를 사용하지 않은 Scaffold 전체 코드

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
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

//수정 요소
class MyHomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: BottomNavigationBar(items: [
        BottomNavigationBarItem(
            icon: Icon(Icons.home),
            title: Text('Home'),
        ),
        BottomNavigationBarItem(
            icon: Icon(Icons.person),
            title: Text('Profile'),
        ),
        BottomNavigationBarItem(
          icon: Icon(Icons.notifications),
          title: Text('Notification'),
        ),
      ]),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-10.png)


---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)
