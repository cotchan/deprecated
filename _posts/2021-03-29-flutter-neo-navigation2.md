---
title: Flutter) [네비게이션2] Clean Navigation without context (GlobalKey 사용)
author: cotchan
date: 2021-03-29 20:10:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ 아래 출처 포스팅 내용(Navigate without context in Flutter with a Navigation Service)를 해석하여 정리한 내용입니다.

---

## 1. GlobalKey란

- **Flutter에서 `GlobalKeys`는 StatefulWidget의 상태에 접근하는데 사용할 수 있습니다.**
- **그래서 `GlobalKeys`를 사용해서 `Build Context 외부에서 NavigatorState에 접근`하는데 사용할 것입니다.**

---

## 2. NavigationService라는 클래스 만들기

- 클래스 초기화 시 해당 `GlobalKey를 설정`
- 또한 주어진 routeName으로 이동할 수 있는 `navigateTo 함수`를 만듭니다.

```dart
//NavigationService.dart

class NavigationService {
  final GlobalKey<NavigatorState> navigatorKey =
      new GlobalKey<NavigatorState>();
  Future<dynamic> navigateTo(String routeName) {
    return navigatorKey.currentState.pushNamed(routeName);
  }
}
```

---

```dart
import 'package:get_it/get_it.dart';

final GetIt locator = GetIt.instance;

void setupLocator() {
  locator.registerLazySingleton(() => NavigationService());
}
```

---

## 3. MaterialApp에 등록하기

+ **main.dart에서 MaterialApp에 Globalkey를 `navigatorkey 프로퍼티에` 제공합니다.**

```dart
@override
Widget build(BuildContext context) {
  return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      navigatorKey: locator<NavigationService>().navigatorKey,
      home: HomeView());
}
```

---

+ **또한 `onGenerateRoute 프로퍼티에` 라우팅 경로를 설정해줍니다.**

```dart
@override
Widget build(BuildContext context) {
 return MaterialApp(
    title: 'Flutter Demo',
    theme: ThemeData(
      primarySwatch: Colors.blue,
    ),
    navigatorKey: navigationService.navigatorKey,
    onGenerateRoute: (routeSettings) {
      switch (routeSettings.name) {
        case 'login':
          return MaterialPageRoute(builder: (context) => LoginView());
        default:
          return MaterialPageRoute(builder: (context) => HomeView());
      }
    },
    home: HomeView());
}
```

---

## 4. 설정 후 Navigation 하는 방법

+ 샘플 페이지 2 개 `HomeView`, `LoginView` 가정

```dart
class HomeView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('HomeView'),
      ),
    );
  }
}

class LoginView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('LoginView'),
      ),
    );
  }
}
```

---

+ **NavigationService 사용해서 Navigation하는 방법**

```dart
class HomeView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          locator<NavigationService>().navigateTo('login');
        },
      ),
      body: Center(
        child: Text('HomeView'),
      ),
    );
  }

  //...
}
```

---

+ 출처
  + [Navigate without context in Flutter with a Navigation Service](https://medium.com/flutter-community/navigate-without-context-in-flutter-with-a-navigation-service-e6d76e880c1c)
