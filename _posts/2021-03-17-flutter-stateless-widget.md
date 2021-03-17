---
title: Flutter) Stateless 위젯
author: cotchan
date: 2021-03-17 21:37:00 +0800
categories: [Flutter,Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Stateless 위젯

+ **Stateless 위젯은 `화면 표시용 위젯`입니다.**
+ 위젯이 로딩되어 화면에 표시된 이후에는 사용자 이벤트나 동작이 있어도 내용을 변경할 수 없습니다.
+ **Stateless 위젯은 `StatelessWidget 클래스` 입니다.**


---

## 2. Stateless Widget 만드는 법

+ Stateless 위젯을 만들기 위해서는
  + **먼저 `StatelessWidget 클래스를 상속`받고**
  + **`build() 메서드에서 내가 원하는 위젯을 반환`하면 됩니다.**

---

## 3. Stateless Widget Sample

```dart
import 'package:flutter/material.dart';

void main() =>
    runApp(MaterialApp(
      title: 'Stateless Widget Demo',
      home: Scaffold(
        appBar: AppBar(title: Text('Stateless 위젯 데모')),
        body: _FirstStatelessWidget(),
      ),
    ));

class _FirstStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('이것은 stateless 위젯입니다.');
  }
}
```

---

![Desktop View](/assets/img/post/flutter/2021-03-17-flutter-stateless-widget.png)

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
