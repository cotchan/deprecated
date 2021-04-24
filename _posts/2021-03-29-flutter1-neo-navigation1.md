---
title: Flutter) [네비게이션2] Clean Navigation with Context (Generated Routes 사용)
author: cotchan
date: 2021-03-29 20:00:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ 아래 출처 포스팅 내용(Clean Navigation in Flutter Using Generated Routes)를 해석하여 정리한 내용입니다.

---

## 1. 플러터에서 Navigation을 사용하는 방법

+ **플러터에서는 Navigation하는 방법이 두 가지 있습니다.**
  + 명시적으로 경로를 설정하여 pushed Routes
  + **`Named Routes` 사용 → 이 방법이 좀 더 바람직합니다.**
    + 그래서 이 포스팅에서는 Named Routes 방법을 설명할 것입니다.

---

## 2. Named Routes 방법 설명

---


## 2-1. onGenerateRoute 프로퍼티

- **MaterialApp 클래스에는 `onGenerateRoute`라는 프로퍼티가 있습니다.**
    - **`이걸 사용할 것임`**

- 이 프로퍼티에는 아래 스펙의 함수를 등록할 수 있습니다.

```dart
//parameter: RouteSettings
//return value: Route<dynamic>

Route<dynamic> func(RouteSettings settings);
```

---

## 2-2. onGenerateRoute에 등록하기 위해 라우팅 함수를 만드는 방법
        
- **라우팅함수는 `Router.generateRoute()을 의미`합니다.**

- **여기서 사용할 방법은 `Route<dynamic>`값을 `MaterialPageRoute` 클래스로 생성할 것입니다.**
- 여기서 이 func 함수를 등록할 때 2 가지 중요한 정보가 있습니다.
    - **첫 번째는 `이름`**
        - **이름을 사용하여** 라우팅 요청이 들어왔을 때 **반환할 뷰를 결정**합니다.
    - **두 번째는 `parameter`**

---

+ 예시 코드 1

```dart
//Router.dart

class Router {

static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
      case '/':
        return MaterialPageRoute(builder: (_) => Home());
      case '/feed':
        return MaterialPageRoute(builder: (_) => Feed());
      default:
        return MaterialPageRoute(
            builder: (_) => Scaffold(
                  body: Center(
                      child: Text('No route defined ${settings.name}')),
                ));
    }
  }
}
```

---

+ 예시 코드 2 (좀 더 정리한 버전)

```dart
//Router.dart

const String homeRoute = '/';
const String feedRoute = 'feed';

class Router {

	static Route<dynamic> generateRoute(RouteSettings settings) {
	
		switch (settings.name) {
		      case homeRoute:
		        return MaterialPageRoute(builder: (_) => Home());
		      case feedRoute:
		        return MaterialPageRoute(builder: (_) => Feed());
	  }
	}
}
```

```dart
NavigatorService.dart

class NavigatorService {

  static const String SPLASH_PAGE_ROUTE = '/';
  static const String LOGIN_PAGE_ROUTE = '/login';
  static const String MAIN_PAGE_ROUTE = '/main';
  static const String FILE_DETAIL_VIEW_ROUTE = '/detail';

  static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
      case SPLASH_PAGE_ROUTE:
        return MaterialPageRoute(builder: (_) => SplashPage());
      case LOGIN_PAGE_ROUTE:
        return MaterialPageRoute(builder: (_) => LoginPage());
      case MAIN_PAGE_ROUTE:
        return MaterialPageRoute(builder: (_) => MainPage());
      case FILE_DETAIL_VIEW_ROUTE:
        List<Map<String,String>> file = settings.arguments as List<Map<String,String>>;
        return MaterialPageRoute(builder: (_) => FileDetailView(file));
    }
  }
}
```

---

## 2-3. Named Routes를 위해 MaterialApp 설정 방법

- 이제 Material App을 정의하는 곳에서 **`Router.generateRoute 함수`를 `onGenerateRoute 프로퍼티로` 넣습니다.**
- 홈뷰 (`Home()`)를 시작 뷰로 정의하기 위해 홈 속성을 위젯을 설정하는 대신 `initialRoute`를 사용합니다.

```dart
//main.dart

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      onGenerateRoute: Router.generateRoute,
      initialRoute: homeRoute,
    );
  }
}
```

---

## 3. Named Routes 설정 후 Navigation 하는 방법

```dart
const String homeRoute = '/';
const String feedRoute = 'feed';

//example
Navigator.pushNamed(context, feedRoute);
```

- 이제 네이게이션을 원할 때 위와 같이 하면 됩니다.
- **위 코드를 사용하면 FeedView로 이동합니다.**

---

## 4. Navigation 시 View에 데이터도 함께 전달하는 방법

- **만약에 FeedView에 매개 변수를 전달하려면 약간의 설정 변경이 필요합니다.**
    - 전달하는 데이터 값은 아무 값이나 전달할 수 있습니다.
- 예시의 경우 FeedView가 문자열을 매개변수로 사용하도록 수정 해보겠습니다.

```dart
//HomeView
class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      floatingActionButton: FloatingActionButton(onPressed: (){
        Navigator.pushNamed(context, feedRoute, arguments: 'Data from home');
      },),
      body: Center(child: Text('Home')),
    );
  }
}


//Router.generateRoute
const String homeRoute = '/';
const String feedRoute = 'feed';

class Router {

	static Route<dynamic> generateRoute(RouteSettings settings) {
	
		switch (settings.name) {
		      case homeRoute:
		        return MaterialPageRoute(builder: (_) => Home());
					case feedRoute:
				    var data = settings.arguments as String;
				    return MaterialPageRoute(builder: (_) => Feed(data));
	  }
	}
}


//FeedView
class Feed extends StatelessWidget {

  final String data;

  Feed(this.data);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(child: Text('Feed: $data')),
    );
  }
}
```

---

+ 출처
  + [Clean Navigation in Flutter Using Generated Routes ](https://www.filledstacks.com/snippet/clean-navigation-in-flutter-using-generated-routes/#navigation)
