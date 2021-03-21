---
title: Flutter) [레이아웃과 위젯] RaisedButton 위젯
author: cotchan
date: 2021-03-21 20:00:00 +0800
categories: [Flutter,Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. RaisedButton 위젯이란

+ **플러터에서 가장 기본적인 버튼**
+ 이외에도 FlatButton, DropdownButton, FloatingActionButton 등이 존재

---

## 2. RaisedButton 위젯 생성자

+ `onPressed` 속성은 반드시 있어야 합니다.
+ 만약 버튼 이벤트를 처리하는 콜백 함수가 없다면 `null`을 넣으면 됩니다.
+ 버튼에 들어가는 내용은 `child` 속성에 넣으면 됩니다.
  + 가장 손쉬운 방법은 child 속성에 Text 위젯을 넣는 것입니다.

```dart
RaisedButton({
  Key key,
  @required VoidCallback onPressed,
  ValueChanged<bool> onHighlightChanged,
  ButtonTextTheme textTheme,
  Color textColor,
  Color disabledTextColor,
  Color color,
  Color disabledColor,
  Color focusColor,
  Color hoverColor,
  Color highlightColor,
  Color splashColor,
  Brightness colorBrightness,
  double elevation,
  double focusElevation,
  double highlightElevation,
  double disabledElevation,
  EdgeInsetsGeometry padding,
  ShapeBorder shape,
  Clip clipBehavior,
  FocusNode focusNode,
  MaterialTapTargetSize materialTapTargetSize,
  Duration animationDuration,
  Widget child,
})
```

---

## 3. RaisedButton 위젯 샘플코드

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

void main() => runApp(ButtonDemo());

class ButtonDemo extends StatefulWidget { //11111
  @override
  State createState() => ButtonDemoState();
}

class ButtonDemoState extends State<ButtonDemo> {
  static const String _title = "Button 위젯 데모";
  String _buttonState = "OFF";  //33333

  void onClick() {
    print('onClick()');
    setState( () {  //44444
      if (_buttonState == 'OFF') {
        _buttonState = 'ON';
      } else {
        _buttonState = 'OFF';
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _title,
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(title: Text(_title)),
        body: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            RaisedButton(
              child: Text('사각 버튼'),
              onPressed: onClick,
            ),
            Text('$_buttonState'),
            RaisedButton(
              child: Text("둥근 버튼"),
              onPressed: onClick, //22222
              shape: RoundedRectangleBorder(
                borderRadius: new BorderRadius.circular(30.0)))
          ],
        ),
      ));
  }
}
```

---

## 4. RaisedButton 위젯 샘플코드 해석

+ **`11111`**
  + ButtonDemo 클래스는 StatelessWidget이 아니라 `StatefulWidget 클래스를 사용`합니다.
  + 그 이유는 버튼을 눌렀을 때 버튼 이벤트에 따라 화면을 갱신하기 위해서입니다.

+ **`22222`**
  + 콜백(callback) 함수로는 onClick 메서드를 지정했습니다.

+ **`33333`**
  + 흥미로운 것은 리액티브 뷰(Reactive View)입니다.
  + 안드로이드를 포함하여 일반적인 UI 프로그래밍에서는 어떤 UI 컴포넌트의 값을 변경할 때 그 컴포넌트의 참조를 받아서 직접 변경합니다.
  + 하지만 플러터에서는 더 깔끔한 방법을 사용합니다. 

+ **`44444`**
  + 버튼 사이에 있는 Text 위젯을 표현하는 방법을 보겠습니다.
  1. 변수 _buttonState를 지정합니다. 초깃값은 'OFF'입니다.
  2. 그리고 사각 버튼과 둥근 버튼을 눌렀을 때 onPressed 이벤트를 받아 onClick 함수를 실행합니다.
  3. onClick 함수에서는 setState() 메서드를 통해 _buttonState 값을 갱신합니다.
  4. 최종적으로 Text 위젯의 값이 변경되었습니다.

---

## 5. 샘플코드 결과화면

![Desktop View](/assets/img/post/flutter/2021-03-21-text-widget-example-3.png)

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
