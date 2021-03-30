---
title: Flutter) [네비게이션] 6.1 새로운 화면으로 이동
author: cotchan
date: 2021-03-29 14:34:00 +0800
categories: [Flutter,Flutter_navigation]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 0. 예제 코드 기본 구조

+ 두 화면을 내비게이션하는 앱
+ 각 화면에는 RaisedButton이 하나씩 있고 이 버튼을 눌렀을 때 화면 전환이 되도록 수정

```dart
//기본 구조
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: FirstPage(), //첫 페이지를 시작 페이지로 지정
    );
  }
}

class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
        onPressed: () {},
      ),
    );
  }
}

class SecondPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이전 페이지로'),
        onPressed: () {},
      ),
    );
  }
}
```

---

## 1. push로 새로운 화면 호출

- FirstPage 클래스를 수정하여 SecondPage로 전환하려면 `Navigator 클래스의 push() 메서드`를 사용합니다.

- 기본적인 사용 방법

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => [이동할 페이지]),
);
```

---

- 첫 번째 인수로 `context`가 필요
- 두 번째 인수로 `MaterialPageRoute 인스턴스`가 필요
- 이 클래스는 머티리얼 디자인으로 작성된 페이지 사이에 화면 전환을 할 때 사용됩니다.
- **이 클래스의 builder 프로퍼티에 `이동할 페이지를 나타내는 함수를 작성`합니다.**
    - 입력 매개변수인 BuildContext 타입은 타입 추론에 의해 생략 가능합니다.
    - (Context context) ⇒ SecondPage()

+ Sample Code
  + 아래 코드는 FirstPage의 버튼을 눌렀을 때 SecondPage로 이동하는 코드

```dart
class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SecondPage()),
          );
        },
      ),
    );
  }
}
```

+ Navigator.push() 메서드의 두 번째 인수로 사용된 MaterialPageRoute 클래스는 안드로이드와 iOS 각 플랫폼에 맞는 화면 전환을 지원해줍니다. 
+ builder 프로퍼티에는 BuildContext 인스턴스를 인수로 받고 이동할 화면의 클래스 인스턴스를 반환하는 함수를 작성합니다. 
  + 여기서는 SecondPage 화면을 표시
+ **별다른 코드를 추가하지 않아도 두 번째 페이지에는 AppBar의 `leading 영역에` `뒤로 가기 아이콘이 표시`됩니다.**
  + 이 아이콘을 탭하면 이전 화면으로 이동하도록 자동으로 구성됩니다.

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-34.png)

---

## 2. pop으로 이전 화면으로 이동

+ **`Navigator.push()` 메서드로 새로운 화면이 표시되어도 `이전 화면은 메모리에 남게 됩니다.`**
+ **이 때 `Navigator.pop()` 메서드로 현재 화면을 종료하고 `이전 화면으로 돌아갈 수 있습니다.`**

+ Sample Code

```dart
class SecondPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이 페이지로'),
        onPressed: () {
          Navigator.pop(context);  //현재 화면을 종료하고 이전 화면으로 돌아가기
        },
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-35.png)

---

## 3. 새로운 화면에 값 전달하기

+ 새로운 화면을 표시하면서 데이터도 함께 전달하는 방법

+ Person 클래스 정의하기

```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);
}

class FirstPage extends StatelessWidget { ...생략... }

class SesondPage extends StatelessWidget { ...생략... }
```

---

+ **FirstPage 클래스의 버튼 클릭 이벤트 부분 수정**

```dart
onPressed: () {
  final person = Person('홍길동', 20);
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondPage(person: person)),
  );
},
```

---

+ **SecondPage 클래스에서 `Person 객체를 받을 수 있도록` 다음과 같이 코드 수정**

```dart
class SecondPage extends StatelessWidget {

  final Person person;

  SecondPage({@required this.person});


  @override
  Widget build(BuildContext context) {
    return ...생략...
  }
}
```

+ **`@required`를 붙이면 필수 입력 인수를 나타냅니다.**
  + **SecondPage 클래스의 생성자는 Person 객체를 반드시 받아야 합니다.**

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: FirstPage(), //첫 페이지를 시작 페이지로 지정
    );
  }
}

class Person {
  String name;
  int age;

  Person(this.name, this.age);
}

class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
        onPressed: () {
          final person = Person('홍길동', 20);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SecondPage(person: person)),
          );
        },
      ),
    );
  }
}

class SecondPage extends StatelessWidget {

  final Person person;

  SecondPage({@required this.person});

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text(person.name),
        onPressed: () {
          Navigator.pop(context);  //현재 화면을 종료하고 이전 화면으로 돌아가기
        },
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-34.png)

---

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-36.png)


---

## 4. 이전 화면으로 데이터 돌려주기

- `Navigator.push()` 메서드와 `Navigator.pop()` 메서드를 수정하면 SecondPage클래스에서 FirstPage 클래스로 데이터를 돌려줄 수 있습니다.
- **다음 코드는 ok라는 문자열을 이전 페이지에 돌려줍니다.**
- SecondPage의 onPressed 메서드를 아래와 같이 수정합니다.

```dart
//ok라는 문자열을 이전 페이지에 돌려주는 코드
Navigator.pop(context, 'ok');

//SecondPage의 onPressed 메서드
onPressed: () {
  Navigator.pop(context, 'ok');  
  //현재 화면을 종료하고 이전 화면으로 돌아가기
  //ok라는 문자열을 이전 페이지에 돌려줍니다.
},
```

---

- **FirstPage 클래스가 SecondPage 클래스로부터 데이터를 돌려받으려면 push() 메서드를 다음과 같이 수정합니다.**

```dart
onPressed: () async {
  final person = Person('홍길동', 20);
  final result = await Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondPage(person: person)),
  );


  print(result);
}
```

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: FirstPage(), //첫 페이지를 시작 페이지로 지정
    );
  }
}

class Person {
  String name;
  int age;

  Person(this.name, this.age);
}

class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First'),
      ),
      body: RaisedButton(
        child: Text('다음 페이지로'),
        onPressed: () async {
          final person = Person('홍길동', 20);
          final result = await Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SecondPage(person: person)),
          );

          print(result);
        }
      ),
    );
  }
}

class SecondPage extends StatelessWidget {

  final Person person;

  SecondPage({@required this.person});

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text('Second'),
      ),
      body: RaisedButton(
        child: Text('이전 페이지로'),
        onPressed: () {
          Navigator.pop(context, 'ok');  //현재 화면을 종료하고 이전 화면으로 돌아가기
        },
      ),
    );
  }
}
```

---

+ 결과 화면
  + 콘솔을 보면 아래와 같이 `print(result)` 값이 찍히는 것을 볼 수 있습니다.

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-37.png)

---

## 4-1. Future, await, async

+ **이전 화면으로 데이터 돌려주기(내용 이어서)**

- push() 메서드는 `Future 타입의 반환 타입`을 가집니다.
- Future는 미래에 값이 들어올 것을 나타내는 클래스입니다.
- **Future 값을 반환받으려면 다음의 두 가지 조치를 해야 합니다.**
    - `await 키워드를 메서드 실행 앞에` 추가합니다.
    - await 키워드를 사용하는 메서드의 인수와 함수 본문 사이에 `async 키워드를 추가`합니다.
- 이러한 코드는 push() 메서드가 어떤 값을 반환할 때까지 기다리게 합니다.
    - 그리고 반환값을 기다리는 동안 앱이 멈추지 않습니다.
    - **나중에 값이 들어오면 그 값이 result에 담긴 후 비로소 print문이 실행됩니다.**
- 이렇게 어떤 일이 끝날 대까지 기다리면서 앱이 멈추지 않도록 하는 방식을 비동기 방식이라고 합니다.



---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

