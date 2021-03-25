---
title: Flutter) [위젯] 4.5 화면 표시용 위젯
author: cotchan
date: 2021-03-25 00:00:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ 버튼과 더불어 화면 구성시 가장 자주 사용되는 위젯인 `텍스트`, `이미지`, `아이콘`, `프로그레스바`에 대해 알아보겠습니다.

---

## 1. Text 

+ 글자를 표시하는 위젯입니다.

+ Text 위젯은 기본적으로
  + **첫 번째 인수에 문자열을 지정하여 Text('글자') 형태로 사용**
  + **style 프로퍼티에 TextStyle 클래스의 인스턴스를 지정하여 다양한 글자 표현 가능**
  + **TextStyle 클래스는 아래 코드와 같이 글자 크기, 색상, 폰트 스타일 등을 쉽게 설정 가능**
+ 참고로 Text 클래스의 `첫 번째 인수는 필수 프로퍼티`고 이름 없는 인수입니다.
+ style 뿐만 아니라 모든 `이름 있는 인수는 옵션` 성격이므로 필요한 것을 선택적으로 사용할 수 있습니다.

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 40.0,  //글자 크기
    fontStyle: FontStyle.italic,  //이탤릭체
    fontWeight: FontWeight.bold,  //볼드체
    color: Colors.red,  //색상
    letterSpacing: 4.0,  //자간
  ),
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
        body: Text(
          'Hello World',
          style: TextStyle(
            fontSize: 40.0,  //글자 크기
            fontStyle: FontStyle.italic,  //이탤릭체
            fontWeight: FontWeight.bold,  //볼드체
            color: Colors.red,  //색상
            letterSpacing: 4.0,  //자간
          ),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-21.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

