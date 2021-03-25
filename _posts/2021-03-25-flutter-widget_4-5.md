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

## 2. Image

+ **이미지를 표시하는 위젯입니다.**
+ 플러터에서는 네트워크에 있는 이미지를 간단히 표시할 수 있습니다.
+ `network()` 메서드에 이미지 파일의 URL을 입력하기만 하면 됩니다.

```dart
Image.network('http://bit.ly/2Pvz4t8')  //이미지 URL
```

---

+ **물론 `asset() 메서드`로 이미지 파일을 직접 표시할 수도 있습니다.**
+ 이미지 파일을 사용하려면 프로젝트 내에 별도의 폴더를 만든 후 이미지 파일을 복사해둡니다.
+ 보통 assets 폴더를 만들고 이미지 파일을 복사해둡니다.
+ **이미지 파일을 사용할 수 있도록 pubspec.yaml 파일을 수정해야 합니다.**
  + pubspec.yaml 파일의 flutter: 항목 아래의 assets: 항목 아래에 폴더명을 지정하면 됩니다.
  + assets/sampleImage.jpg와 같은 형태로 나열해도 되지만, assets/ 와 같이 전체 폴더를 가리키면 여러 항목을 매 번 작성할 필요가 없습니다.

```dart
flutter:
  assets:
    - assets/
```

---

+ **pubspec.yaml을 수정한 후에는 터미널에서 `flutter packages get` 명령을 실행하여 프로젝트에서 이미지 파일에 접근할 수 있게 해야 합니다.**
+ 그리고 나서 아래와 같이 이미지 파일을 사용할 수 있습니다.

```dart
Image.asset('assets/sample.jpg')
```


---


## 3. Icon

+ 아이콘 위젯은 단독으로도 사용하지만 메뉴나 리스트, 버튼과의 조합으로 사용합니다.
+ 머티리얼 디자인용 기본 아이콘들은 Icons 클래스에 상수로 미리 정의되어 있습니다.
+ Icon 클래스의 첫 번째 인수에 Colors 클래스에 미리 정의된 다양한 머티리얼 아이콘을 지정합니다.
  + 색상이나 크기 등을 자유롭게 지정할 수 있습니다.

```dart
Icon(
  Icons.home,
  color: Colors.red,
  size:60.0,  //기본값 24.0	
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
        body: Icon(
          Icons.home,
          color: Colors.red,
          size:60.0,  //기본값 24.0
        ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-22.png)


---

## 4. Progress

+ 로딩 중이거나 오래 걸리는 작업을 할 때 사용자에게 진행 중임을 보여주는 용도로 사용하는 위젯입니다.
+ 두 종류를 제공합니다.
  + **`CircularProgressIndicator()`는 둥근 형태의 프로그레스바입니다.**
  + **`LinearProgressIndicator()`는 선 형태의 프로그레스바입니다.**
+ 일반적으로 다른 화면 위에 겹쳐서 표시하므로 `Stack 위젯으로 겹쳐서 사용`합니다.

---

## 5. CircleAvatar

+ 프로필 화면 등에 많이 사용되는 원형 위젯입니다.
+ child 프로퍼티에 정의한 위젯을 원형으로 만들어줍니다.

```dart
CircleAvatar(
  child: Icon(Icons.person),
),
```

---

+ 네트워크상에 존재하는 이미지를 표시한다면
  + child 프로퍼티가 아닌 backgroundImage 프로퍼티에 NetworkImage 클래스의 인스턴스를 지정해야 네트워크에서 받아온 이미지가 원형으로 표시됩니다.

```dart
CircleAvatar(
  backgroundImage: NetworkImage([이미지 URL]),
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
        body: Row(
          mainAxisSize: MainAxisSize.max,
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            CircleAvatar(
              child: Icon(Icons.person),
            ),
          ],
        )
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-23.png)


---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

