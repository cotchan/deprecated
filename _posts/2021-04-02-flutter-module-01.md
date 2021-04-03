---
title: Flutter) [Module] 입력값 출력하기
author: cotchan
date: 2021-04-02 20:44:00 +0800
categories: [Flutter, Flutter_module]
tags: [flutter_module]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. TextField, TextFormField 

+ 사용자에게 값을 입력받을 때 사용하는 위젯
  + `TextField` 위젯
  + `TextFormField` 위젯

---

## 2. TextEditingController 

+ **`TextEditingController` 클래스 인스턴스를 통해서 TextField 위젯에 작성된 값을 얻을 수 있습니다.**

---

## 3. 코드 예시

+ 예시코드
  + TextField 위젯이 2개 있고, 입력값이 변하면 각각 로그를 출력

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Retrieve Text Input',
      home: MyCustomForm(),
    );
  }
}

class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {

  //TextField의 현재값을 얻는 데 필요합니다.
  final myController = TextEditingController();    //11111

  void initState() {
    super.initState();

    //addListener로 상태를 모니터링 할 수 있음
    myController.addListener(_printLatestValue);    //44444
  }


  _printLatestValue() {     //55555
    // 컨트롤러의 text 프로퍼티로 연결된 TextField에 입력된 값을 얻음
    print("두 번째 Text field: ${myController.text}");
  }

  @override
  void dispose() {
    //화면이 종료될 때는 반드시 위젯 트리에서 컨트롤러를 해제해야 합니다.
    myController.dispose();    //22222
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Text Input 연습'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: <Widget>[
            TextField(
              onChanged: (text) {     //66666
                print("첫 번째 text field: $text");
              },
            ),
            TextField(
              controller: myController, //33333 controller 지정
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## 4. 코드 해석

+ `11111`
  + `TextField 위젯 수에 맞게` TextEditingController를 준비합니다.

+ `22222`
  + **컨트롤러는 화면이 종료될 때 `dispose() 메서드로 반드시 해제`해야 합니다.**

+ **`33333`**
  + TextField 위젯에서는 `controller 프로퍼티`에 컨트롤러 변수를 설정합니다.
  + **이렇게 하면 `myController 컨트롤러 변수를 통해 TextField 인스턴스의 값을 얻거나 변경된 값을 모니터링` 할 수 있습니다.**

+ **`44444`**
  + **TextField 위젯의 값이 변경될 때마다 무언가를 수행하고 싶다면 `addListener() 메서드를 사용`합니다.**
  + myController 컨트롤러 변수가 연결된 두 번째 TextField의 값이 변경될 때마다 `_printLatestValue() 메서드가 실행`됩니다.

+ `55555`
  + 컨트롤러가 연결된 TextField 위젯에 입력된 값은 `컨트롤러의 text 프로퍼티`를 통해 얻을 수 있습니다.

+ **`66666`**
  + **`TextField 위젯 자체의 값 변경을 모니터`링하는 것은 `onChanged 프로퍼티로도 구현가능`합니다.**

---

## 5. 결과값

![Desktop View](/assets/img/post/flutter/2021-04-02-module-01.png)

---

![Desktop View](/assets/img/post/flutter/2021-04-02-module-02.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

