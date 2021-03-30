---
title: Flutter) [위젯] 5.3 이벤트
author: cotchan
date: 2021-03-29 13:59:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ `onTap`, `onPressed` 등의 이벤트를 기본 프로퍼티로 가지고 있지 않은 위젯에 이벤트를 적용할  수 있도록 해주는 위젯입니다.

---

## 1. GestureDetector와 InkWell

+ 글자나 그림같이 이벤트 프로퍼티가 없는 위젯에 이벤트를 적용하고 싶을 때 사용하는 위젯

- `GestureDetector`와 `InkWell` 위젯은 **`터치 이벤트를 발생`**시킵니다.
- onTap 프로퍼티를 가지고 있어서 child 프로퍼티에 어떠한 위젯이 와도 클릭 이벤트를 작성할 수 있습니다.
- 따라서 Text, Image 등의 위젯에도 간단히 클릭 이벤트를 추가할 수 있습니다.
- InkWell 위젯으로 감싸고 클릭하면 물결 효과가 나타나지만 GestureDetector 위젯은 그렇지 않습니다.

```dart
//Sample1
GestureDetector(
  onTap: () {
    //클릭 시 실행
  },
  child: [위젯],
),

InkWell(
  onTap: () {
    //클릭 시 실행
  },
  child: [위젯],
),
```

---

```dart
//Sample2
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    GestureDetector(
      onTap: () {
        print('GestureDetector 클릭!!');
      },
      child: Text('클릭 Me!!'),
    ),
    SizedBox(
      height: 40,
    ),
    InkWell(
      onTap: () {
        print('InkWell 클릭!!');
      },
      child: Text('클릭 Me!!'),
    ),
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
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          GestureDetector(
            onTap: () {
              print('GestureDetector 클릭!!');
            },
            child: Text('클릭 Me!!'),
          ),
          SizedBox(
            height: 40,
          ),
          InkWell(
            onTap: () {
              print('InkWell 클릭!!');
            },
            child: Text('클릭 Me!!'),
          ),
        ],
      ),
    );
  }
}

```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-33.png)

---

+ **`클릭 Me!!`를 클릭하면 아래와 같은 Event Log가 남습니다.**

```
I/flutter (31306): GestureDetector 클릭!!
I/flutter (31306): GestureDetector 클릭!!
I/flutter (31306): GestureDetector 클릭!!
I/flutter (31306): InkWell 클릭!!
I/flutter (31306): InkWell 클릭!!
I/flutter (31306): InkWell 클릭!!
```

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

