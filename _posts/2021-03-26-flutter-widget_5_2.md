---
title: Flutter) [위젯] 5.2 다이얼로그
author: cotchan
date: 2021-03-26 08:32:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ 다이얼로그는 사용자의 확인을 요구하거나 메시지를 표시하는 용도로 자주 사용합니다.

---

## 1. AlertDialog

- 머티리얼 디자인의 유저 확인용 다이얼로그입니다.
- AlertDialog를 표시하려면 `showDialog()함수의 builder 프로퍼티`에 `AlertDialog 클래스의 인스턴스를 반환하는 함수`를 작성하면 됩니다.
- showDialog() 함수의 barrierDismissible 프로퍼티는 다이얼로그 바깥 부분을 탭(터치)해도 닫히게 할 것인지 정합니다.

```dart
showDialog(
  context: context,
  barrierDismissible: false,  //사용자가 다이얼로그 바깥을 터치하면 닫히지 않음
  builder: (BuildContext context) {
    return AlertDialog(
      title: Text('제목'),
      content: SingleChildScrollView(
        child: ListBody(
          children: <Widget>[
            Text('Alert Dialog입니다'),
            Text('OK를 눌러 닫습니다'),
          ],
        ),
      ),
      actions: <Widget>[
        FlatButton(
          child: Text('OK'),
          onPressed: () {
            //다이얼로그 닫기
            Navigator.of(context).pop();
          },
        ),
        FlatButton(
          child: Text('Cancel'),
          onPressed: () {
            //다이얼로그 닫기
            Navigator.of(context).pop();
          },
        ),
      ],
    );
  },
);
```

---

- AlertDialog 클래스는 `title`과 `content`, `actions` 영역을 정의해줍니다.
- title은 제목 영역입니다.
- content는 내용 영역입니다.
- 예제 코드에서는 SingleChildScrollView와 ListBody 클래스를 사용하여 ListView와 동일한 효과를 가지도록 했습니다.
- action 프로퍼티에는 버튼들을 정의합니다. 예제에서는 FlatButton 두 개를 정의했고 각 버튼을 클릭할 때 Navigator.of(context).pop()을 호출하여 다이얼로그를 닫습니다.


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
class MyHomePage extends StatefulWidget {

  @override
  _MyHomePageState createState() => _MyHomePageState();
}


class _MyHomePageState extends State<MyHomePage> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Show AlertDialog'),
      ),
      body: Center(
        child:RaisedButton(
          child: Text("Show AlertDialog"),
          onPressed: () {
            _showDialog(context);
          },
        )
      )
    );
  }
}

void _showDialog(BuildContext context) {
  showDialog(
    context: context,
    barrierDismissible: false,  //사용자가 다이얼로그 바깥을 터치하면 닫히지 않음
    builder: (BuildContext context) {
      return AlertDialog(
        title: Text('제목'),
        content: SingleChildScrollView(
          child: ListBody(
            children: <Widget>[
              Text('Alert Dialog입니다'),
              Text('OK를 눌러 닫습니다'),
            ],
          ),
        ),
        actions: <Widget>[
          FlatButton(
            child: Text('OK'),
            onPressed: () {
              //다이얼로그 닫기
              Navigator.of(context).pop();
            },
          ),
          FlatButton(
            child: Text('Cancel'),
            onPressed: () {
              //다이얼로그 닫기
              Navigator.of(context).pop();
            },
          ),
        ],
      );
    },
  );
}
```

---

+ 결과 화면
  + 초기 상태

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-31.png)

---

+ 결과 화면
  + Dialog 클릭 시

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-32.png)

---

## 2. DatePicker

+ 날짜를 선택할 때 사용합니다.

---

## 3. TimePicker

+ 시간을 선택할 때 사용하는 위젯입니다.


---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

