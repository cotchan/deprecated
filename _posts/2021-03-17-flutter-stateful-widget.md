---
title: Flutter) Stateful 위젯
author: cotchan
date: 2021-03-17 21:51:00 +0800
categories: [Flutter,Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Stateful 위젯

+ **Stateful 위젯은 버튼을 누르거나 터치했을 때 화면에 변경된 내용을 표시할 수 있습니다.**
+ **`Stateful 위젯은 상태를 가집니다.`**
  + 새로 만든 _FirstStatefulWidget 클래스는 build() 메서드 대신에
    + **상태를 생성하는 `createState()` 메서드를 구현합니다.**
    + 이 메서드는 _FirstStateWidgetState 객체를 반환합니다.

+ `_FirstStatefulWidgetState` 클래스는 `State<_FirstStatefulWidget>` 클래스를 상속했습니다.
  + **`이 클래스의 역할은 _FirstStatefulWidget 클래스의 상태를 담당합니다.`**

+ 그리고 나서 화면에 위젯을 표시하는 build() 메서드를 구현하게 됩니다.

```dart
//stateless_to_stateful_widget_demo.dart

import 'package:flutter/material.dart';

void main() =>
    runApp(MaterialApp(
      title: 'Hello Flutter',
      home: Scaffold(
        appBar: AppBar(title: Text('Stateless -> Stateful 위젯 데모')),
        body: _FirstStatefulWidget(),
      ),
    ));

//고정부
class _FirstStatefulWidget extends StatefulWidget {
  @override
  State createState() => _FirstStatefulWidgetState();
}

//변동부
class _FirstStatefulWidgetState extends State<_FirstStatefulWidget> {
  @override
  Widget build(BuildContext context) {
    return Text('이것은 stateful 위젯입니다.');
  }
}
```

---

![Desktop View](/assets/img/post/flutter/2021-03-17-flutter-stateful-widget.png)

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
