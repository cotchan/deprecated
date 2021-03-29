---
title: Flutter) [네비게이션] 6.2 routes를 활용한 내비게이션
author: cotchan
date: 2021-03-29 19:57:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. routes 정의

- routes는 **MaterialApp 클래스의 `routes 프로퍼티`에 다음과 같은 형태로 정의**할 수 있습니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: FirstPage(),  //첫 페이지를 시작 페이지로 고정
      routes: {
        '/first': (context) => FirstPage(),
        '/second': (context) => SecondPage(),
      },
    );
  }
}
```

- routes 프로퍼티에 Map 형태로 (키-값 쌍으로) 문자열과 목적지 인스턴스를 작성하면 됩니다.
- `/first`는 FirstPage클래스로, `/second`는 SecondPage 클래스로 연결되도록 정의 

---

## 2. routes에 의한 화면 이동

+ 이제 기존의 `push()` 메서드 대신 `pushNamed()` 메서드를 사용하여 화면 내비게이션을 실행시킬 수 있습니다.
+ FirstPage 클래스의 화면 이동 부분을 다음과 같이 수정합니다.

```dart
onPressed: () async {
  final result = await Navigator.pushNamed(context, '/second');

  print(result);
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
      home: FirstPage(),  //첫 페이지를 시작 페이지로 고정
      routes: {
        '/first': (context) => FirstPage(),
        '/second': (context) => SecondPage(),
      },
    );
  }
}

class Person {
  String name;
  int age;

  Person(this.name, this.age);
}

class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
          onPressed: () async {
            final result = await Navigator.pushNamed(context, '/second');

            print(result);
          }
      ),
    );
  }
}

class SecondPage extends StatelessWidget {

  final Person person;

  SecondPage({@required this.person});

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이전 페이지로'),
        onPressed: () {
          Navigator.pop(context, 'ok');  //현재 화면을 종료하고 이전 화면으로 돌아가기
        },
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-38.png)

---

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-39.png)

---

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-40.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

