---
title: Flutter) Stateful 위젯의 생명주기
author: cotchan
date: 2021-03-21 15:31:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 생명주기란

+ 안드로이드 네이티브에 비해 플러터의 `Stateless 위젯`은 생명주기가 없습니다.
  + 오직 화면에 표시만 하는 역할입니다.
+ **`Stateful 위젯`만 State 클래스를 통해 생명주기를 지원합니다.**

---

## 2. Stateful 위젯의 생명주기 메서드

---

## 2-1. initState()

+ **`중요`**
+ **상태를 초기화할 수 있습니다.**
+ **단 한번만 호출됩니다.**
+ 유용한 함수이므로 반드시 알아두어야 합니다.

```dart
@override
//1. Stateful 위젯이 생성될 때 한 번만 호출됩니다.
//보통 필요한 변수를 초기화할 때 사용합니다.
void initState() {

}
```

---

## 2-2. build()

+ **`중요`**
+ **`(필수)`위젯을 화면에 표시하는 메서드**
+ **화면에 표시할 위젯을 반환해야 합니다.**

```dart
@override
//2. 위젯의 내용을 반환합니다.
//setState() 함수가 호출되면 build() 메서드가 언제든지 재실행될 수 있습니다.
//따라서 계산이 오래 걸리는 동작은 이 안에 포함되어 있으면 안 됩니다.
Widget build(BuildContext context) {
  return Text('이것은 stateful 위젯입니다.');
}
```

---

## 2-3. setState()

+ **`중요`**
+ **setState() 메서드는 전달된 익명 함수를 실행한 후 화면을 다시 그리게 하는 역할을 합니다.**
  + 화면은 `build()` 메서드가 실행되면서 그려진다고 위에서 말했습니다.
  + **즉, `setState() 메서드는 build() 메서드가 다시 실행되게 하는 역할`을 합니다.**

+ **위젯의 상태를 갱신합니다.**
+ setState() 메서드는 State 클래스가 제공하는 메서드입니다.
+ 이 메서드를 실행하면 위젯을 처음부터 다시 만들지만 initState 메서드는 호출되지 않습니다.

```dart
@override
//3. 화면을 갱신할 때 호출합니다.
//인자로 넘기는 fn 함수에 변경하기 원하는 내용을 넣습니다.
void setState(VoidCallback fn) {

}
```

---

## 2-4. StatefulWidget.createState()

+ 상태를 생성합니다.
+ 이 메서드를 제외하고 나머지는 모두 State 클래스에 있습니다.

---


## 2-5. didChangeDependencies()

+ 상태 객체의 의존성이 변경되면 호출됩니다.

---

## 2-6. didUpdateWidget(()

+ 위젯의 설정이 변경될 때 호출됩니다.

---

## 2-7. deactivate()

+ 상태 객체가 위젯 트리에서 제거됩니다.
+ 경우에 따라 다시 위젯 트리에 추가될 수도 있습니다.

---

## 2-8. dispose()

+ 상태 객체가 위젯 트리에서 완전히 제거됩니다.
+ 이 메서드가 호출되면 상태 객체는 더 이상 재사용할 수 없습니다.

---

## 2-9. mounted 변수가 true가 됨

+ 화면에 위젯이 부착된 상태

---

## 2-10. mounted 변수가 false로 설정

+ 최종적으로 위젯이 화면에서 탈착됩니다.

---

## 3. LifeCycle SampleCode

```dart
import 'package:flutter/material.dart';

void main() => runApp(MaterialApp(
  title: 'Stateful 위젯 데모',
  home: Scaffold(
    appBar: AppBar(title: Text('Stateful 위젯 데모')),
    body: _MyStatefulWidget(),
  ),
));

//고정부
class _MyStatefulWidget extends StatefulWidget {
  @override
  State createState() => _MyStatefulWidgetState();
}

//변동부
class _MyStatefulWidgetState extends State<_MyStatefulWidget> {

  String _buttonState;

  @override
  //1. Stateful 위젯이 생성될 때 한 번만 호출됩니다.
  //보통 필요한 변수를 초기화할 때 사용합니다.
  void initState() {
    super.initState();
    print('initState(): 기본값을 설정합니다.');
    _buttonState = 'OFF';
  }

  @override
  void didChangeDependencies() {
    print('didChangeDependencies() 호출됨');
  }

  @override
  //2. 위젯의 내용을 반환합니다.
  //setState() 함수가 호출되면 build() 메서드가 언제든지 재실행될 수 있습니다.
  Widget build(BuildContext context) {
    print('build() 호출됨');
    return Column(
      children: <Widget>[
        RaisedButton(
          child: Text('버튼을 누르세요'),
          onPressed: _onClick,
        ),
        Row(
          children: <Widget>[
            Text('버튼 상태: '),
            Text(_buttonState),
          ],
        )
      ],
    );
  }

  void _onClick() {
    print('_onClick() 호출됨');

    //3. 화면을 갱신할 때 호출합니다.
    //인자로 넘기는 fn 함수에 변경하기 원하는 내용을 넣습니다.
    setState(() {
      print('setState() 호출됨');
      if (_buttonState == 'OFF') {
        _buttonState = 'ON';
      } else {
        _buttonState = 'OFF';
      }
    });
  }

  @override
  void didUpdateWidget(_MyStatefulWidget oldWidget) {
    print('didUpdateWidget()');
  }

  @override
  void deactivate() {
    print('deactivate()');
  }

  @override
  void dispose() {
    print('disposie()');
  }
}
```

---

## 4. SampleCode 실행 로그

+ **첫 번째 화면 `초기화 시점`**

```
I/flutter (10603): initState(): 기본값을 설정합니다.
I/flutter (10603): didChangeDependencies() 호출됨
I/flutter (10603): build() 호출됨
``` 

---

+ **`버튼 클릭 시`**

```
//첫 번째 클릭
I/flutter (10603): _onClick() 호출됨
I/flutter (10603): setState() 호출됨
I/flutter (10603): build() 호출됨

//두 번째 클릭
I/flutter (10603): _onClick() 호출됨
I/flutter (10603): setState() 호출됨
I/flutter (10603): build() 호출됨
```

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
