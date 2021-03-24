---
title: Flutter) [위젯] 4.4 버튼 계열 위젯
author: cotchan
date: 2021-03-25 00:00:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ 플러터는 여러 종류의 버튼 위젯을 제공합니다. 그 중에 가장 많이 사용되는 버튼

---

## 0. 버튼 계열 위젯 특징

+ **버튼 위젯들은 모두 `onPressed 프로퍼티`에 버튼이 클릭되었을 때 실행될 함수를 반드시 정의해줘야 버튼이 활성화되며 클릭 가능합니다.**
+ **`만약 null을 지정`하면 버튼이 클릭되지 않는 `비활성화 상태`가 됩니다.**

---

## 1. RaisedButton

+ 입체감을 가지는 일반적인 버튼 위젯

```dart
RaisedButton(
  child: Text('RaisedButton'),
  color: Colors.orange,
  onPressed: (){
    //실행될 코드 작성
  },
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
        body: RaisedButton(
          child: Text('RaisedButton'),
          color: Colors.orange,
          onPressed: (){
            //실행될 코드 작성
          },
        ),
    );
  }
}


```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-17.png)

---

## 2. FlatButton

+ 평평한 형태의 버튼입니다.

```dart
FlatButton(
  child: Text('FlatButton'),
  onPressed: () {},
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
        body: FlatButton(
          child: Text('FlatButton'),
          color: Colors.yellow,
          onPressed: () {},
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-18.png)

---

## 3. IconButton

+ 아이콘을 표시하는 위젯입니다.
+ 아이콘의 크기나 색을 지정할 수 있습니다.
+ **이 위젯은 다른 위젯과 다르게 자식 위젯을 포함할 수 없기 때문에 `child 프로퍼티가 없습니다.`**
+ 대신 아이콘을 `icon 프로퍼티`에 작성하고 크기는 `iconSize 프로퍼티`로 설정합니다.

```dart
IconButton(
  icon: Icon(Icons.add),
  color: Colors.red,  //아이콘 색상
  iconSize: 100.0,  //기본값 24.0
  onPressed: () {},
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
        body: IconButton(
          icon: Icon(Icons.add),
          color: Colors.red,  //아이콘 색상
          iconSize: 100.0,  //기본값 24.0
          onPressed: () {},
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-19.png)

---

## 4. FloatingActionButton

+ 입체감 있는 둥근 버튼 위젯입니다. 아이콘을 표시하는 데 사용합니다.
+ **Scaffold의 floatingActionButton 프로퍼티에 바로 사용할 수 있습니다.**
+ **또한 일반적인 버튼처럼 단독으로 사용할 수도 있습니다.**

```dart
FloatingActionButton(
  child: Icon(Icons.add),
  onPressed: () {},
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
        body: FloatingActionButton(
          child: Icon(Icons.add),
          onPressed: () {},
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-20.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

