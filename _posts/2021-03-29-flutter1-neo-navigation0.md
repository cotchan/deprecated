---
title: Flutter) [네비게이션2] Navigator Class (공식 홈페이지 내용)
author: cotchan
date: 2021-03-29 19:59:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ Flutter 공식 홈페이지에서 `Navigator Class` 읽으면서 이해한 내용 바탕으로 작성하였니다. 

---

## 1. Navigation & Route란


- **Flutter에서는 Route 역시 위젯입니다.**
- 새로운 Route로 이동하는 방법 → Navigator 사용해서 가능합니다.

```dart
class Navigator extends StatefulWidget {
  //...
```

+ **Navigator란 스택 원칙으로 하위 위젯 세트를 관리하는 위젯입니다.**
+ **모바일은 page라는 전체 화면 요소를 통해 콘텐츠를 표시함, Flutter에서는 이런 page 들을 Routes라 부릅니다.** 
+ 그리고 Routes는 Navigator 위젯에 의해 관리됩니다.
  + The navigator manages a stack of Route objects.


---

## 2. 네비게이터가 Routes 객체를 관리하는 방법 

- **네비게이터가 Routes 객체들을 관리하는 두 가지 방법이 있습니다.**
  1. the declarative API Navigator.pages
  2. Navigator.push and Navigator.pop.

- 가장 일반적으로 네비게이터를 생성하는 방식은 WidgetsApp이나 MaterialApp 위젯에 의해서 생성된 네비게이터를 사용하는 게 가장 일반적인 방식
    - **그래서 이렇게 위젯에다가 `Navigator.of.`를 갖다꼽으면 → 해당 위젯(WidgetsApp, MaterialApp)에 대한 `NavigatorState가 리턴`됩니다.**
    - 그리고 이렇게 만들면 MaterialApp Widget이 Navigator 스택의 맨 아래가 됩니다.
      - 즉 전환될 화면들 간의 관계에서 가장 아래에 있는 애가 됨

```dart
//공식 홈페이지의 Static Method of 정의
of(BuildContext context, {bool rootNavigator: false}) → NavigatorState
//The state from the closest instance of this class that encloses the given context
```

---

+ **새로운 Route 객체를 생성해서 stack에 푸시하려면 `MaterialPageRoute 객체를 생성`하면 됩니다.**

- 라우터라는 놈은 `자신이 가지는 위젯`을 builder 함수를 사용해서 정의합니다.
    - **child widget으로 정의하는 게 X**
- **라우터라는 놈은 `자신이 사용하는 위젯`을 builder 함수를 사용해서 정의합니다.**
    - child widget으로 정의하는 게  X
- 왜냐면 푸시되고 팝됨에 따라서 계속해서 다른 위젯과 매핑이 되기 때문입니다.
    - 라우트는 푸시와 팝되는 시기에 따라 다른 컨텍스트에서 다시 빌드되기 때문입니다.

- 앱의 홈페이지 경로 이름은 기본적으로 '/' 입니다.
    - The app's home page route is named '/' by default.

- **머티리얼앱을 `라우터네임 → Builder function`을 매핑한 Map을 가지고 만들 수 있습니다.**
- 머리티얼앱은 위 Map을 사용하여 자신이 쓸 네비게이터의 `onGenerateRoute 콜백`을 생성합니다.

---

```dart
void main() {
  runApp(MaterialApp(
    home: MyAppHome(), // becomes the route named '/'
    routes: <String, WidgetBuilder> {
      '/a': (BuildContext context) => MyPage(title: 'page A'),
      '/b': (BuildContext context) => MyPage(title: 'page B'),
      '/c': (BuildContext context) => MyPage(title: 'page C'),
    },
  ));
}

Navigator.pushNamed(context, '/b');

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Code Sample for Navigator',
      // MaterialApp contains our top-level Navigator
      initialRoute: '/',
      routes: {
        '/': (BuildContext context) => HomePage(),
        '/signup': (BuildContext context) => SignUpPage(),
      },
    );
  }
}
```

---

## 3. 라우터에 return value를 사용할 거라면

+ 만약에 라우터를 `return value를 위해 사용`한다면 라우터 타입에 return value가 들어가 있어야 합니다.

```dart
//pop할 때 bool을 리턴할 거라면
MaterialPageRoute<bool>

//pop할 때 bool을 리턴할 거라면
MaterialPageRoute<void>
```

---

+ 출처
  + [Navigator class](https://api.flutter.dev/flutter/widgets/Navigator-class.html)
