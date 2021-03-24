---
title: Flutter) [위젯] 4.3 위치, 정렬, 크기를 위한 위젯
author: cotchan
date: 2021-03-24 20:40:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

+ 화면을 구성할 때는 배치한 위젯의 위치를 정해야 합니다.
+ 위젯 중에는 크기나 위치, 정렬 등을 보조하는 위젯이 있습니다.
+ **아래의 경우에 필요한 위젯을 알아봅니다.**
  + `위젯을 중앙에 배치`하거나
  + `한쪽 방향으로 정렬`하거나
  + `위젯 사이에 여백`을 주거나
  + `위젯을 특정 크기로` 만들고 싶을 때 

---

## 1. Center

+ 중앙으로 정렬시키는 위젯입니다.
+ **상당히 자주 사용하는 위젯입니다.**

```dart
Center(
  child: [위젯],	
)
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
        body: Center(
          child: Container(
            color: Colors.red,
            width: 100,
            height: 100,
          ),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-11.png)

---

## 2. Padding

+ **안쪽 여백을 표현할 때 사용하는 위젯입니다.**
+ 안쪽 여백은 padding 프로퍼티에 값을 지정합니다.
+ **이 값은 `EdgeInsets의 클래스를 사용`하여 설정하며 다음과 같이 여러 방법을 제공합니다.**
+ 앞에 const를 붙이면 컴파일 타임에 상수로 정의되어 다시 사용되는 부분이 있을 경우 메모리에 있는 값을 재사용하는 이득이 있습니다.

```dart
Padding(
  padding: const EdgeInsets.all(40.0),
  child: [위젯],
),
```

## 2-1. EdgeInsets

+ EdgeInsets는 여러 함수를 제공합니다.
+ `all()` 함수는 네 방향 모두 같은 값을 지정합니다.
+ EdgeInsets.all([double])
+ `only()` 함수는 상하좌우 중에서 `원하는 방향에만 값을 지정`합니다. 지정하지 않은 방향에는 기본값 0.0이 지정됩니다.
  + EdgeInsets.only({left: [왼쪽], top: [위], right: [오른쪽], bottom: [아래]})
+ `fromLTRB()` 함수는 네 방향의 값을 각각 지정합니다.
  + EdgeInsets.fromLTRB([왼쪽], [위], [오른쪽] ,[아래])

---

+ Padding Sample Code

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
        body: Padding(
          padding: const EdgeInsets.all(40.0),  //빨간 사각형의 사방에 40 만큼 패딩 적용
          child: Container(
            color: Colors.red,
          ),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-12.png)

---

## 3. Align

+ 자식 위젯의 정렬 방향을 정할 수 있는 위젯입니다.
+ 원하는 방향으로 위젯을 정렬할 때 사용합니다.
+ **자식 위젯을 정렬하기 위해서는 `alignment 프로퍼티`에 정렬하고자 하는 방향을 정의해야 합니다.**

```dart
Align(
  alignment: Alignment.bottomRight,
  child: [위젯],	
),
```

---

+ **alignment 프로퍼티에 정의할 수 있는 값들은 Alignment 클래스에 정의되어 있습니다.**

+ `bottomLeft`: 하단 왼쪽
+ `bottomCenter`: 하단 중앙
+ `bottomRight`: 하단 오른쪽
+ `centerLeft`: 중앙 왼쪽
+ `center`: 중단 중앙
+ `centerRight`: 중단 오른쪽
+ `topLeft`: 상단 왼쪽
+ `topCenter`: 상단 중앙
+ `topRight`: 상단 오른쪽

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
        body: Align(
          alignment: Alignment.bottomRight,
          child: Container(
            color: Colors.red,
            width: 100,
            height: 100,
          ),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-13.png)


---

## 4. Expanded

+ **자식 위젯의 크기를 최대한으로 확장시켜주는 위젯입니다.**
+ 여러 위젯에 동시에 적용하면 `flex 프로퍼티`에 정숫값을 지정하여 비율을 정할 수 있으며 기본 값은 1입니다.

```dart
Column(
  children: <Widget>[
    Expanded(
      flex: [비율], //기본값은 1
      child: [위젯],
    ),
    Expanded(
      //flex 프로퍼티 값을 안 줬으므로 1로 셋팅
      child: [위젯],
    ),
    Expanded(
      //flex 프로퍼티 값을 안 줬으므로 1로 셋팅
      child: [위젯],
    ),
  ]	
)
```

---

+ Sample Code
  + 빨강, 초록, 파랑 박스를 2:1:1로 수직 배치

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
        body: Column(
          children: <Widget>[
            Expanded(
              flex: 2,
              child: Container(
                color: Colors.red,
              ),
            ),
            Expanded(
              child: Container(
                color: Colors.green,
              ),
            ),
            Expanded(
              child: Container(
                color: Colors.blue,
              ),
            ),
          ],
        )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-14.png)


---

## 5. SizedBox

+ 위젯 중에는 크기에 관련된 프로퍼티가 없는 위젯이 많은데 그러한 **`위젯을 특정 크기로 만들고 싶을 때` 사용합니다.**
+ `width`에 가로 길이, `height`에 세로 길이를 `double 타입`으로 지정합니다.
+ SizedBox를 child 없이 단독으로 사용하면 `단순히 여백을 표현`하는데 사용할 수 있습니다.
+ 그냥 Container에 길이를 직접 지정하면 코드가 더 간결해지지만 대부분의 위젯은 크기 지정 프로퍼티를 가지고 있지 않기 때문에 SizedBox를 많이 사용합니다.

```dart
SizedBox(
  width: [가로 길이],
  height: [세로 길이],
  child: [위젯],	
),
```

---

+ Sample Code
  + 빨간 박스를 100, 100 크기로 만들기

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
        body: SizedBox(
          width: 100,
          height: 100,
          child: Container(
            color: Colors.red,
          ),
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-15.png)


---

## 6. Card

+ 카드 형태의 모양을 제공하는 위젯입니다.
+ 기본적으로 크기가 0이므로 자식 위젯의 크기에 따라 결정됩니다.

```dart
Card(
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(16.0),
  ),
  elevation: [실숫값],  //그림자 깊이
  child: [위젯],
),
```

+ **`elevation 프로퍼티`를 지정하여 그림자의 깊이를 조정**할 수 있습니다.
  + 좀 더 깊은 그림자를 표현하려면 좀 더 큰 값을 지정합니다.
+ **`shape 프로퍼티`는 카드 모양을 변경하는 방법을 제공**하며 여기서는 RoundedRectangleBorder 클래스의 인스턴스를 지정했습니다.
  + 이 클래스는 borderRadius 프로퍼티에 BorderRadius.circular() 메서드를 지정하여 카드 모서리의 둥근 정도를 실숫값으로 조절합니다. 값이 클수록 더 둥글게 됩니다.

---

+ Sample Code
  + 카드 위젯을 화면 중앙에 표시하기
  + **중앙에 정렬된 위젯은 아래와 같이 Center 위젯으로 감싼 것으로 보면 됩니다.**
    + 중앙에 표시되는 위젯의 코드에서 `Center 위젯은 생략가능`

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
        body: Center(
          child: Card(
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16.0),
            ),
            elevation: 4.0,  //그림자 깊이
            child: Container(
              width: 200,
              height: 200,
            ),
          ),
        )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-24-widget-16.png)


---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

