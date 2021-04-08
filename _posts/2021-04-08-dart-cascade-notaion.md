---
title: dart) ..연산자(Cascade notation)
author: cotchan
date: 2021-04-08 22:30:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. ..연산자 의미

+ `Cascades 연산자(..)`를 사용하면 **`동일한 개체에 대해 일련의 작업을 수행` 할 수 있습니다.**
+  Cascades 표기법 (..)은 단계 수와 임시 변수의 필요성을 줄여주는 메소드 체인과 유사합니다.
  + 그래서 dart에서 `빌더 패턴을 구현하는 경우`에 사용하면 좋습니다.

---

## 2. 예시 코드

```dart
class Demo {
  var a;
  var b;
  void setA(x) { 
    this.a = x;
  } 
 
  void setB(y) { 
    this.b = y;
  } 
  void showVal(){
    print(this.a);
    print(this.b);
  }
}  
void main() { 
  
  Demo d1 = new Demo(); 
  Demo d2 = new Demo();
  
  print("W3Adda -  Dart Cascade Notation");
  // Without Cascade Notation
  d1.setA(20);
  d1.setB(25);
  d1.showVal();
  // With Cascade Notation
  d2..setA(10) 
    ..setB(15)
    ..showVal();  
}
```

---

+ 출처
  + [Dart Cascade notation(..) Operator](https://www.w3adda.com/dart-tutorial/dart-cascade-notation)

