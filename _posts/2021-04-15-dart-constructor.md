---
title: dart) 생성자
author: cotchan
date: 2021-04-15 15:43:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. this & 기본 생성자

+ **`this 키워드`는 `현재 인스턴스를 참조`합니다.**
+ **생성자를 지정하지 않으면 기본 생성자를 제공합니다.**
+ **기본 생성자는 인수가 없으며 `수퍼 클래스에서 인수가 없는 생성자를 호출`합니다.**

---

## 2. 생성자 파라미터 { }

+ **생성자를 만들때 `{} 로 감싸고 각 멤버변수들을 초기화시켜줍니다.`** 
+ **그러면 매개변수를 `optional` 하게 만들 수 있습니다.**

```dart
import 'dart:math'; 

class Rectangle { 
  int width; 
  int height; 
  Point origin; 
  Rectangle({this.origin = const Point(0,0), this.width = 0, this.height = 0}); 
  
  @override String toString() => 'origin x: ${origin.x} origin y : ${origin.y} width : $width height : $height'; 
} 

main() { 
  print(Rectangle(origin: const Point(10, 20), width: 10, height: 20)); 
  print(Rectangle(origin: const Point(10, 20), width: 10)); 
  print(Rectangle(width: 10)); print(Rectangle(height: 10)); 
  print(Rectangle()); 
}
```

---

+ 코드의 결과값 

```
origin x: 10 origin y : 20 width : 10 height : 20
origin x: 10 origin y : 20 width : 10 height : 0
origin x: 0 origin y : 0 width : 10 height : 0
origin x: 0 origin y : 0 width : 0 height : 10
origin x: 0 origin y : 0 width : 0 height : 0
```

---

## 3. 인스턴스 변수에 생성자 인수 할당

+ dart 언어는 parameter로 받는 인스턴스 변수를 생성자 인수로 넘기는 것을 표현하는 고유한 방식이 있습니다.

```dart
class Point {
  num x, y;

  // x,y 에 각각 값을 할당한다는 의미입니다.
  Point(this.x, this.y);
}
```

+ 기존의 자바 방식
  + **`위의 코드와 같은 의미`입니다.**

```dart
class Point {
  num x, y;

  Point(num x, num y) {
    this.x = x;
    this.y = y;
  }
}
```

---

## 4. 명명된 생성자

+ 명명 된 생성자를 사용하여 클래스에 대한 여러 생성자를 구현하거나 추가적인 명확성을 제공할 수 있습니다.

```dart
class Point {
  num x, y;

  // 일반 생성자
  Point(this.x, this.y);

  // 명명된 생성자 
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```

---

## 5. 목록 초기화

+ `:(콜론)`을 통해 본문의 생성자가 실행되기 전에 값을 초기화할 수 있습니다.


```dart
// :(콜론)을 통해 본문의 생성자가 실행되기 전에 값을 초기화 할 수 있습니다.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
```

---

## 6. 팩토리 생성자

+ **항상 클래스의 새 인스턴스를 생성하지는 않는 생성자를 구현할 때는 `factory 키워드를 사용` 바랍니다.**
+ 예를 들어 팩토리 생성자는 캐시에서 인스턴스를 반환하거나 하위 유형의 인스턴스를 반환 할 수 있습니다.
+ **팩토리 생성자는 `this`로 접근할 수 없습니다.**

+ 사용 예시

```dart
var logger = Logger('UI');
logger.log('Button clicked');
```

---

+ 구현

```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache는  이름 앞에 _ (언더스코어)를 붙여 라이브러리 프라이빗 이다.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

---

## 7. 생성자 리다이렉팅
 
+ 특정 생성자에게 **처리를 `위임(delegating)` 할 수 있습니다.**

```dart
class Point {
  num x, y;

  // 메인 생성자 
  Point(this.x, this.y);

  // 메인 생성자에게 위임 
  Point.alongXAxis(num x) : this(x, 0);
}
```


---

## 8. 생성자는 상속받지 않습니다.

+ **서브 클래스는 슈퍼 클래스에서 생성자를 상속받지 않습니다.**
+ 생성자(constructor)를 선언하지 않는 서브 클래스는, 디폴트 (인수 없음, 이름 없음)의 constructor 만을 가집니다.

---

+ 출처
  + [Flutter - dart로 클래스 생성자 파라미터를 optional 하게 만들어보기](https://duzi077.tistory.com/295)
  + [[FLUTTER] DART 언어 기초과정 - 3 / A Tour of the Dart Language](https://steemit.com/dart/@wonsama/flutter-dart-3-a-tour-of-the-dart-language)
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

