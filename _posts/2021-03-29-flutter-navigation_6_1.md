---
title: Flutter) [네비게이션] 6.1 새로운 화면으로 이동
author: cotchan
date: 2021-03-29 14:34:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 0. 예제 코드 기본 구조

+ 두 화면을 내비게이션하는 앱
+ 각 화면에는 RaisedButton이 하나씩 있고 이 버튼을 눌렀을 때 화면 전환이 되도록 수정

```dart
//기본 구조
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
      home: FirstPage(), //첫 페이지를 시작 페이지로 지정
    );
  }
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
        onPressed: () {},
      ),
    );
  }
}

class SecondPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이전 페이지로'),
        onPressed: () {},
      ),
    );
  }
}
```

---

## 1. push로 새로운 화면 호출

- FirstPage 클래스를 수정하여 SecondPage로 전환하려면 `Navigator 클래스의 push() 메서드`를 사용합니다.

- 기본적인 사용 방법

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => [이동할 페이지]),
);
```

---

- 첫 번째 인수로 `context`가 필요
- 두 번째 인수로 `MaterialPageRoute 인스턴스`가 필요
- 이 클래스는 머티리얼 디자인으로 작성된 페이지 사이에 화면 전환을 할 때 사용됩니다.
- **이 클래스의 builder 프로퍼티에 `이동할 페이지를 나타내는 함수를 작성`합니다.**
    - 입력 매개변수인 BuildContext 타입은 타입 추론에 의해 생략 가능합니다.
    - (Context context) ⇒ SecondPage()

+ Sample Code
  + 아래 코드는 FirstPage의 버튼을 눌렀을 때 SecondPage로 이동하는 코드

```dart
class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SecondPage()),
          );
        },
      ),
    );
  }
}
```

+ Navigator.push() 메서드의 두 번째 인수로 사용된 MaterialPageRoute 클래스는 안드로이드와 iOS 각 플랫폼에 맞는 화면 전환을 지원해줍니다. 
+ builder 프로퍼티에는 BuildContext 인스턴스를 인수로 받고 이동할 화면의 클래스 인스턴스를 반환하는 함수를 작성합니다. 
  + 여기서는 SecondPage 화면을 표시
+ **별다른 코드를 추가하지 않아도 두 번째 페이지에는 AppBar의 `leading 영역에` `뒤로 가기 아이콘이 표시`됩니다.**
  + 이 아이콘을 탭하면 이전 화면으로 이동하도록 자동으로 구성됩니다.

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-34.png)

---

## 2. pop으로 이전 화면으로 이동

+ **`Navigator.push()` 메서드로 새로운 화면이 표시되어도 `이전 화면은 메모리에 남게 됩니다.`**
+ **이 때 `Navigator.pop()` 메서드로 현재 화면을 종료하고 `이전 화면으로 돌아갈 수 있습니다.`**

+ Sample Code

```dart
class SecondPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이 페이지로'),
        onPressed: () {
          Navigator.pop(context);  //현재 화면을 종료하고 이전 화면으로 돌아가기
        },
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-35.png)


---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

