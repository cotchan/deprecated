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

## 1. 생성자 파라미터 { }

+ **생성자를 만들때 `{} 로 감싸고 각 멤버변수들을 초기화시켜줍니다.`** 
+ 그러면 매개변수를 `optional` 하게 만들 수 있습니다.**

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

+ 출처
  + [Flutter - dart로 클래스 생성자 파라미터를 optional 하게 만들어보기](https://duzi077.tistory.com/295)

